<template>
	<view class="components-form">
		<!-- 页面内容 -->
		<view>

			<view :tips="tips" :sectionList="sectionList" :full="true" :fullWindowsScroll="true" @click="click">
				<tn-form :model="model" ref="form" :errorType="errorType" :labelPosition="labelPosition"
					:labelWidth="labelWidth" :labelAlign="labelAlign">
					<tn-form-item label="名称" prop="name" :required="true" :labelPosition="labelPosition"
						:labelAlign="labelAlign">
						<tn-input v-model="model.name" type="text" placeholder="请输入名称" :border="border"></tn-input>
					</tn-form-item>

					<!-- <tn-form-item label="介绍" :required="true" prop="desc" :labelPosition="labelPosition"
						:labelAlign="labelAlign">
						<tn-input v-model="model.desc" type="textarea" placeholder="请输入介绍" :border="border"
							inputAlign="center"></tn-input>
					</tn-form-item> -->
					<tn-form-item label="密码" prop="password" :labelPosition="labelPosition" :labelAlign="labelAlign">
						<tn-input v-model="model.password" type="password" placeholder="请输入密码" :border="border"
							:passwordIcon="true"></tn-input>
					</tn-form-item>
					<tn-form-item label="确认密码" prop="rePassword" :labelPosition="labelPosition"
						:labelAlign="labelAlign">
						<tn-input v-model="model.rePassword" type="password" placeholder="请再次输入密码" :border="border">
						</tn-input>
					</tn-form-item>
					<tn-form-item label="上传图片" prop="photo" :labelPosition="labelPosition" :labelAlign="labelAlign">
						<tn-image-upload :fileList="model.photo" :action="action" @on-list-change="imageUploadChange"
							:autoUpload="true" @on-success="upsuccess()"></tn-image-upload>
					</tn-form-item>
				</tn-form>
				<!-- 				<view class="agreement">
					<tn-checkbox v-model="model.agreement" @change="agreementCheckChange"></tn-checkbox>
					<view class="agreement-text">勾选同意当前协议</view>
				</view> -->
				<tn-button backgroundColor="#01BEFF" fontColor="#FFFFFF" width="100%" @click="submit">提交</tn-button>
			</view>


			</tn-action-sheet>
			<!-- 地区picker -->
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
					remember: false,
					photo: "",
					agreement: false
				},
				rules: {
					name: [{
							required: true,
							message: '请输入用户名',
							trigger: 'blur'
						},
						{
							min: 3,
							max: 16,
							message: '3到16位（字母，数字，下划线，减号）',
							trigger: ['change', 'blur'],
						}, {
							// 正则表达式验证演示
							pattern: /^[a-zA-Z0-9_-]{3,16}$/,
							message: '3到16位（字母，数字，下划线，减号）',
							trigger: 'change'
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
							pattern: /^[a-zA-Z0-9_-]{3,16}$/,
							message: '3到16位（字母，数字，下划线，减号）',
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
		onLoad() {},
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
						console.log(that.$data.model)
						const requestTask2 = uni.request({
							url: 'http://www.rural.abc/user/update', //仅为示例，并非真实接口地址。
							method: 'POST',
							header: {
								'token': wx.getStorageSync('token'), //自定义请求头信息
								'content-type': "application/x-www-form-urlencoded"
							},
							data: {

								name: this.$data.model.name,
								password: this.$data.model.password,
								image: this.$data.model.image,
							},
							success: function(res) {
								// con
								console.log(res)
								if (res.data == "修改成功！") {
									that.$refs.toast.show({
										title: '更新成功',
										content: '',
										icon: 'success',
										image: '',
										duration: 1500
									});
									uni.reLaunch({
										url: '/pages/login/login'
									});
								} else {
									that.$refs.toast.show({
										title: '发布失败',
										content: '请稍后再试',
										icon: 'close-circle',
										image: '',
										duration: 1500
									})
								}

							},
							fail: function(res) {
								that.$refs.toast.show({
									title: '更新失败',
									content: '请检查网络情况',
									icon: 'close-circle',
									image: '',
									duration: 1500
								})
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
			// 获取验证码
			getCode() {
				if (this.$refs.code.canGetCode) {
					this.$tn.message.loading('正在获取验证码')
					setTimeout(() => {
						this.$tn.message.closeLoading()
						this.$tn.message.toast('验证码已经发送')
						// 通知组件开始计时
						this.$refs.code.start()
					}, 2000)
				} else {
					this.$tn.message.toast(this.$refs.code.secNum + '秒后再重试')
				}
			},
			// 验证码倒计时时间改变
			codeChange(text) {
				this.codeTips = text
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