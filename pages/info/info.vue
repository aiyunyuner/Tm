<template>
	<view>
		<view style="margin: 1ch; ">
			<cover-image :src="sightInfor.image"></cover-image>
		</view>
		<view>
			<view :class="[`tn-bg-${sightInfor.color}--light tn-color-${sightInfor.color}`] "
				style="text-align: center;margin: 1ch;">
				简 介
			</view>
			<view style="margin: 1ch;">
				{{sightInfor.desc}}
			</view>
		</view>
		<view style="margin: 1ch;text-align: center;"
			:class="[`tn-bg-${sightInfor.color}--light tn-color-${sightInfor.color}`] ">地 点</view>
		<view style="margin: 1ch;text-align: center;">
			{{sightInfor.where}}
		</view>
		<view style="margin: 1ch;text-align: center;"
			:class="[`tn-bg-${sightInfor.color}--light tn-color-${sightInfor.color}`] ">其 他</view>
		<view style="margin: 1ch;text-align: center;">
			访 问 量：{{sightInfor.click}} 收 藏 量：{{sightInfor.likeCount}}
		</view>


		<demo-title title="评论">
			<view class="content tn-padding-left tn-padding-right">
				<tn-form-item label="发表新评论" :labelWidth="200">
					<tn-input type="textarea" v-model="newtype" placeholder="发表新评论文字"></tn-input>
					<tn-button @click="newComment()" slot="right" backgroundColor="#01BEFF" fontColor="#FFFFFF"
						size="sm">发表</tn-button>
				</tn-form-item>
			</view>
			<tn-list-view unlined="bottom" v-for="(item,index) in sightsList" :key="index">

				<tn-list-cell :unlined="true">
					<view class="message">
						<view class="message__left">
							<tn-avatar :src="item.user.image"></tn-avatar>
						</view>
						<view class="message__middle">
							<view class="message__name">{{item.user.name}}</view>
							<view class="message__content tn-text-ellipsis">{{item.context}}</view>
						</view>
						<view class="message__right">
							<view class="message__time">{{item.time}}</view>
							<view class="message__tips">
								<tn-tag backgroundColor="tn-bg-red" fontColor="tn-color-white" shape="circle"
									width="auto" size="sm">0</tn-tag>
							</view>
						</view>
					</view>
				</tn-list-cell>
			</tn-list-view>
		</demo-title>
		<tn-toast ref="toast"></tn-toast>
	</view>
</template>

<script>
	import demoTitle from '@/libs/components/demo-title.vue'
	export default {
		name: 'componentsList',
		components: {
			demoTitle
		},
		data() {
			return {
				p_id: "",
				sightsList: [],
				total: "", //总共多少条数据
				formData: {
					pageSize: 2, //每页10条数据
					page: 1, //第几页
				},
				newtype: "",
				token: '',
				sightInfor: '',
			}
		},
		onReachBottom() {
			let that = this
			let allTotal = this.formData.page * this.formData.pageSize
			if (allTotal <= this.total) {
				//当前条数小于总条数 则增加请求页数
				that.$data.formData.page++;
				this.getData() //调用加载数据方法
			} else {
				// console.log('已加载全部数据')
			}
		},
		//接受前面传来的参数
		onLoad: function(e) {
			console.log(e.sid)
			this.$data.formData.page = 1;

			let a = e.sid;
			this.$data.p_id = e.sid;
			let that = this

			uni.request({
				url: "http://www.rural.abc/sights/getSightsById", //仅为示例，并非真实接口地址。
				method: 'POST',
				data: {
					id: a,
				},
				header: {
					'token': wx.getStorageSync('token'), //自定义请求头信息
					'content-type': "application/x-www-form-urlencoded"
				},
				success: function(res) {
					that.$data.sightInfor = res.data
				}
			});

			this.getalldate();
		},
		methods: {
			getalldate() {
				this.$data.formData.page = 1;
				let that = this
				uni.request({
					url: 'http://www.rural.abc/sights/commentCunt',
					method: 'POST',
					header: {
						'token': wx.getStorageSync('token'), //自定义请求头信息
						'content-type': "application/x-www-form-urlencoded"
					},
					data: {
						id: that.$data.p_id
					},
					success: function(res) {

						that.$data.total = res.data
					}
				});
				uni.request({
					url: "http://www.rural.abc/sights/getCommentAll",
					method: 'POST',
					data: {
						start: that.$data.formData.page,
						pageSize: that.$data.formData.pageSize,
						p_id: that.$data.p_id,
					},
					header: {
						'token': wx.getStorageSync('token'), //自定义请求头信息
						'content-type': "application/x-www-form-urlencoded"
					},
					success: function(res) {
						//请求数据成功
						console.log(res)
						if (res.data.length != 0) {
							// 	//新数据push到列表中
							that.$data.sightsList = res.data
							let los = that.$data.sightsList
							for (var i = 0; i < los.length; i++) {
								los[i].time = los[i].time.substring(11)
							}
						}
					}
				})
			},
			getData() {

				let that = this
				uni.request({
					url: 'http://www.rural.abc/sights/commentCunt',
					method: 'POST',
					header: {
						'token': wx.getStorageSync('token'), //自定义请求头信息
						'content-type': "application/x-www-form-urlencoded"
					},
					data: {
						id: that.$data.p_id
					},
					success: function(res) {

						that.$data.total = res.data
					}
				});
				uni.request({
					url: "http://www.rural.abc/sights/getCommentAll",
					method: 'POST',
					data: {
						start: that.$data.formData.page,
						pageSize: that.$data.formData.pageSize,
						p_id: that.$data.p_id,
					},
					header: {
						'token': wx.getStorageSync('token'), //自定义请求头信息
						'content-type': "application/x-www-form-urlencoded"
					},
					success: function(res) {
						//请求数据成功
						console.log(res)
						if (res.data.length != 0) {
							// 	//新数据push到列表中
							const newlist = res.data

							for (var i = 0; i < newlist.length; i++) {
								newlist[i].time = newlist[i].time.substring(11)
							}
							that.$data.sightsList.push(...newlist)
						}
					}
				})

			},
			getComment() {
				let that = this;
				uni.request({
					url: "http://www.rural.abc/sights/getCommentAll",
					method: 'POST',
					data: {
						p_id: that.$data.sightInfor.id,
						context: that.$data.newComment
					},
					header: {
						'token': wx.getStorageSync('token'), //自定义请求头信息
						'content-type': "application/x-www-form-urlencoded"
					},
					success: function(res) {
						if (res == 1) {
							that.$refs.toast.show({
								title: '发布成功',
								content: '',
								icon: 'success',
								image: '',
								duration: 1500
							})

						} else {
							that.$refs.toast.show({
								title: '发布失败',
								content: '请稍后再试',
								icon: 'close-circle',
								image: '',
								duration: 1500
							})
						}
					}
				})
			},
			newComment() {
				let that = this;
				uni.request({
					url: "http://www.rural.abc/sights/insertComment",
					method: 'POST',
					data: {
						p_id: that.$data.sightInfor.id,
						context: that.$data.newtype
					},
					header: {
						'token': wx.getStorageSync('token'), //自定义请求头信息
						'content-type': "application/x-www-form-urlencoded"
					},
					success: function(res) {
						if (res.data == 1) {
							that.$refs.toast.show({
								title: '发布成功',
								content: '',
								icon: 'success',
								image: '',
								duration: 1500
							})

							that.getalldate();
						} else {
							that.$refs.toast.show({
								title: '发布失败',
								content: '请稍后再试',
								icon: 'close-circle',
								image: '',
								duration: 1500
							})
						}
					}
				})
			}
		}
	}
</script>

<style lang="scss" scoped>
	.components-list {
		background-color: $tn-bg-gray-color;
		min-height: 100vh;
	}

	.custom-title {
		display: flex;
		align-items: flex-end;
		justify-content: flex-end;
		padding: 10rpx 20rpx;
	}

	.list__options {
		display: flex;
		align-items: center;
		justify-content: space-between;
	}

	.list {
		&__left {
			display: flex;
			align-items: center;
			justify-content: flex-start;

			&__icon,
			&__image {
				margin-right: 18rpx;
			}
		}

		&__right {
			display: flex;
			align-items: center;
			justify-content: flex-end;
		}
	}

	.list-icon-text,
	.list-image-text {
		display: flex;
		align-items: center;
		justify-content: space-between;
	}

	.list-image-text {
		.list {
			&__left {
				&__image {
					width: 20rpx;
					height: 20rpx;
				}
			}
		}
	}

	.message {
		display: flex;
		flex-direction: row;
		align-items: center;
		justify-content: space-around;

		&__left {
			width: 10%;
		}

		&__middle {
			width: 80%;
			padding-left: 20rpx;
			padding-right: 40rpx;
		}

		&__right {
			width: 10%;
			display: flex;
			flex-direction: column;
			align-items: center;
			justify-content: center;
		}

		&__name {
			font-size: 32rpx;
			margin-bottom: 8rpx;
		}

		&__content {
			font-size: 26rpx;
			color: #838383;
		}

		&__tips {
			&__icon {
				font-size: 36rpx;
				color: #AAAAAA;
			}
		}
	}
</style>