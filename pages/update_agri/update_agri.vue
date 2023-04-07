<template>
	<view class="components-form">

		<!-- 顶部自定义导航 -->

		<tn-form-item prop="image">
			<image :src="model.image"></image>
		</tn-form-item>
		<!-- 页面内容 -->
		<view>
			<view :tips="tips" :sectionList="sectionList" :full="true" :fullWindowsScroll="true" @click="click">
				<tn-form :model="model" ref="form" :errorType="errorType" :labelPosition="labelPosition"
					:labelWidth="labelWidth" :labelAlign="labelAlign">
					<tn-form-item label="名称" prop="name" :required="true" :labelPosition="labelPosition"
						:labelAlign="labelAlign">
						<tn-input v-model="model.name" type="text" placeholder="请输入名称" :border="border"></tn-input>
					</tn-form-item>
					<tn-form-item label="介绍" :required="true" prop="desc" :labelPosition="labelPosition"
						:labelAlign="labelAlign">
						<tn-input v-model="model.desc" type="textarea" placeholder="请输入介绍" :border="border"
							inputAlign="center"></tn-input>
					</tn-form-item>
					<tn-form-item label="详细地址" prop="where" :required="true" :labelPosition="labelPosition"
						:labelAlign="labelAlign">
						<tn-input v-model="model.where" type="text" placeholder="请输入详细地址" :border="border"></tn-input>
					</tn-form-item>

					<tn-form-item label="标签颜色" prop="color" :labelPosition="labelPosition" :labelAlign="labelAlign">
						<tn-input v-model="model.color" type="select" placeholder="请选择标签颜色" :border="border"
							:selectOpen="selectShow2" @click="selectShow2 = true"></tn-input>
					</tn-form-item>
					<tn-form-item label="类型" prop="type" :labelPosition="labelPosition" :labelAlign="labelAlign">
						<tn-input v-model="model.type" type="text" placeholder="请输入活动类型" :border="border"></tn-input>
					</tn-form-item>
					<tn-form-item label="上传图片" prop="photo" :labelPosition="labelPosition" :labelAlign="labelAlign">
						<tn-image-upload :fileList="model.photo" :action="action" @on-list-change="imageUploadChange"
							:autoUpload="true" @on-success="upsuccess()"></tn-image-upload>
					</tn-form-item>
				</tn-form>

				<tn-button backgroundColor="#01BEFF" fontColor="#FFFFFF" width="100%" @click="submit">提交</tn-button>
			</view>
			</tn-action-sheet>
			<!-- 地区picker -->
			<tn-picker v-model="pickerShow" mode="region" @confirm="regionPickerConfirm"></tn-picker>
			<!-- 商品类型select -->
			<tn-select v-model="selectShow" mode="single" valueName="type_id" labelName="name" :list="selectListType"
				@confirm="goodsTypeSelectConfirm">
			</tn-select>
			<tn-select v-model="selectShow2" mode="single" labelName="name" :list="selectColor"
				@confirm="selectColorConfirm">
			</tn-select>
		</view>
		<tn-toast ref="toast"></tn-toast>
	</view>
</template>

<script>
	export default {

		data() {
			return {
				action: 'http://www.rural.abc/img/upload',
				selectColor: [{
					name: "red"
				}, {
					name: "green"
				}, {
					name: "cyan"
				}, {
					name: "orange"
				}, {
					name: "purplered"
				}, {
					name: "purple"
				}, {
					name: "brown"
				}, ],
				selectListType: [],
				errorType: ['message', 'border-bottom', 'toast'],
				labelPosition: 'left',
				labelAlign: 'right',
				border: false,
				actionSheetShow: false,
				labelWidth: 140,
				checkboxWidth: 'auto',
				checkboxWrap: false,
				radioWidth: 'auto',
				radioWrap: false,
				pickerShow: false,
				selectShow: false,
				selectShow2: false,
				codeTips: '获取验证码',
				checkboxList: [{
						name: '苹果',
						disabled: false
					},
					{
						name: '橘子',
						disabled: false
					},
					{
						name: '香蕉',
						disabled: false
					},
					{
						name: '榴莲',
						disabled: true
					}
				],
				radioList: [{
						name: '微信',
						disabled: false
					},
					{
						name: '支付宝',
						disabled: true
					},
					{
						name: '云闪付',
						disabled: false
					}
				],
				actionSheetList: [{
						text: '男'
					},
					{
						text: '女'
					},
					{
						text: '保密'
					}
				],
				model: {
					type: "",
					name: '',
					sex: '',
					phone: '',
					color: "",
					desc: '',
					password: '',
					rePassword: '',
					fruit: ['橘子'],
					payType: '微信',
					region: '',
					where: "",
					whereto: "",
					code: '',
					remember: false,
					photo: [],
					agreement: false,
					image: "",
				},
				rules: {
					name: [{
							required: true,
							message: '请输入用户名',
							trigger: 'blur'
						},
						{
							min: 3,
							max: 5,
							message: '姓名长度在3到5个字符',
							trigger: ['change', 'blur'],
						},
						{
							// 此为同步验证，可以直接返回true或者false，如果是异步验证，稍微不同，见下方说明
							validator: (rule, value, callback) => {
								return this.$tn.test.chinese(value);
							},
							message: '姓名必须为中文',
							// 触发器可以同时用blur和change，二者之间用英文逗号隔开
							trigger: ['change', 'blur'],
						},
						{
							// 异步验证需要通过调用callback()，并且在里面抛出new Error()
							// 抛出的内容为需要提示的信息，和其他方式的message属性的提示一样
							asyncValidator: (rule, value, callback) => {
								if (value === '图鸟') {
									callback(new Error('姓名重复'));
								} else {
									// 没有错误，也要执行callback()回调
									callback();
								}
							},
							trigger: ['blur'],
						}
					],
					sex: [{
						required: true,
						message: '请选择性别',
						trigger: 'change'
					}],
					phone: [{
						required: true,
						message: '请输入手机号码',
						trigger: 'change'
					}],
					desc: [{
							min: 5,
							message: '简介不能少于5个字',
							trigger: 'change'
						},
						{
							// 正则表达式验证演示
							pattern: /^[\u4e00-\u9fa5]+$/gi,
							message: '简介只能包含中文',
							trigger: 'change'
						}
					],
					password: [{
							required: true,
							message: '请输入密码',
							trigger: ['change', 'blur']
						},
						{
							pattern: /^(?![0-9]+$)(?![a-zA-Z]+$)[0-9A-Za-z]+\S{5,12}$/,
							message: '需同时含有字母和数字，长度在6-12之间',
							trigger: ['change', 'blur']
						}
					],
					rePassword: [{
							required: true,
							message: '请再次输入密码',
							trigger: ['change', 'blur']
						},
						{
							validator: (rule, value, callback) => {
								return value === this.model.password;
							},
							message: '两次输入的密码不相等',
							trigger: ['change', 'blur'],
						}
					],
					fruit: [{
						required: true,
						message: '请选择水果',
						trigger: 'change',
						type: 'array'
					}],
					payType: [{
						required: true,
						message: '请选择支付方式',
						trigger: 'change'
					}],
					region: [{
						required: true,
						message: '所在地区不能为空',
						trigger: 'change'
					}],
					goodsType: [{
						required: true,
						message: '商品类型不能为空',
						trigger: 'change'
					}],
					code: [{
						required: true,
						message: '验证码不能为空',
						trigger: 'change'
					}],
					remember: [{
						required: true,
						message: '记住密码不能为空',
						trigger: 'change'
					}],
					photo: [{
						required: true,
						message: '请选择图片',
						trigger: 'change',
						type: 'array'
					}],
				},

				tips: ['无需依赖额外的样式文件', '使用tn-toast组件'],
				sectionList: [{
					name: '参数切换',
					section: [{
							title: 'label显示位置',
							optional: ['左边', '上边'],
							methods: 'labelPositionChange'
						},
						{
							title: 'label对齐方式',
							optional: ['左对齐', '右对齐', '居中'],
							methods: 'labelAlignChange',
							current: 1
						},
						{
							title: '边框显示',
							optional: ['显示', '隐藏'],
							methods: 'borderChange',
							current: 1
						},
						{
							title: '可选项排列方式',
							optional: ['默认', '宽度的50%', '换行'],
							methods: 'checkRadioStyleChange'
						},
						{
							title: '错误提示方式',
							optional: ['message', 'toast', '下划线', '输入框'],
							methods: 'errorTypeChange'
						}
					]
				}]
			}
		},
		onReady() {
			this.$refs.form.setRules(this.rules)
		},
		onLoad(res) {
			console.log(res)
			let that = this
			uni.request({
				url: 'http://www.rural.abc/agricultural/one', //仅为示例，并非真实接口地址。
				method: 'POST',
				header: {
					'token': wx.getStorageSync('token'), //自定义请求头信息
					'content-type': "application/x-www-form-urlencoded"
				},
				data: {
					id: res.e
				},

				success: function(res) {
					// console.log(res)
					that.$data.model = res.data
				}
			});
		},
		methods: {
			upsuccess(res) {
				// console.log(res)
				this.$data.model.image = res
			},
			click(event) {
				this[event.methods] && this[event.methods](event)
			},
			// 切换label显示位置
			labelPositionChange(event) {
				switch (event.index) {
					case 0:
						this.labelPosition = 'left'
						break
					case 1:
						this.labelPosition = 'top'
						break
				}
			},
			// 切换label对其方式
			labelAlignChange(event) {
				switch (event.index) {
					case 0:
						this.labelAlign = 'left'
						break
					case 1:
						this.labelAlign = 'right'
						break
					case 2:
						this.labelAlign = 'center'
						break
				}
			},
			// 切换边框显示
			borderChange(event) {
				this.border = event.index === 0 ? true : false
			},
			// 切换可选项样式
			checkRadioStyleChange(event) {
				switch (event.index) {
					case 0:
						this.checkboxWidth = 'auto'
						this.checkboxWrap = false
						this.radioWidth = 'auto'
						this.radioWrap = false
						break
					case 1:
						this.checkboxWidth = '50%'
						this.checkboxWrap = false
						this.radioWidth = '50%'
						this.radioWrap = false
						break
					case 2:
						this.checkboxWidth = 'auto'
						this.checkboxWrap = true
						this.radioWidth = 'auto'
						this.radioWrap = true
						break
				}
			},
			// 切换错误提示方式
			errorTypeChange(event) {
				switch (event.index) {
					case 0:
						this.errorType = ['message']
						break
					case 1:
						this.errorType = ['toast']
						break
					case 2:
						this.errorType = ['border-bottom']
						break
					case 3:
						this.errorType = ['border']
						break
				}
			},


			// 表单提交
			submit() {
				let that = this;
				this.$refs.form.validate(valid => {
					if (valid) {
						uni.request({
							url: 'http://www.rural.abc/agricultural/update', //仅为示例，并非真实接口地址。
							method: 'POST',
							header: {
								'token': wx.getStorageSync('token'), //自定义请求头信息
								'content-type': "application/x-www-form-urlencoded"
							},
							data: {
								id: this.$data.model.id,
								name: this.$data.model.name,
								where: this.$data.model.where,
								type: this.$data.model.type,
								image: this.$data.model.image,
								desc: this.$data.model.desc,
								color: this.$data.model.color,
							},
							success: function(res) {
								// con
								console.log(res)
								if (res.data != 0) {
									that.$refs.toast.show({
										title: '更新成功',
										content: '',
										icon: 'success',
										image: '',
										duration: 1500
									})
									uni.navigateBack();
								} else {
									that.$refs.toast.show({
										title: '更新失败',
										content: '请稍后再试',
										icon: 'close-circle',
										image: '',
										duration: 1500
									})
								}

							}
						});

					} else {
						// 验证失败
					}
				})
			},
			// 点击actionSheet选择性别
			actionSheetClick(index) {
				uni.hideKeyboard()
				this.model.sex = this.actionSheetList[index].text
			},
			// 点击地区选择器
			regionPickerConfirm(event) {
				this.model.region = event.province.label + event.city.label + event.area.label
			},
			// 点击商品类型列选择器
			goodsTypeSelectConfirm(event) {

				this.model.type = `${event[0]['value']}`

			},
			selectColorConfirm(event) {

				this.model.color = `${event[0]['label']}`
			},
			// 多选项值改变事件
			checkboxGroupChange(event) {
				this.model.fruit = event
			},
			// 单选项值改变事件
			radioGroupChange(event) {
				this.model.payType = event
			},

			// 图片有修改
			imageUploadChange(lists) {
				this.model.photo = lists
			},
			// 同意协议状态修改
			agreementCheckChange(event) {
				this.model.agreement = event.value
			}
		}
	}
</script>

<style lang="scss" scoped>
	.agreement {
		display: flex;
		align-items: center;
		margin: 40rpx 0;

		&-text {
			padding-left: 8rpx;
			color: $tn-font-sub-color;
		}
	}
</style>