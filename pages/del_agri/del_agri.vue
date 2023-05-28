<template>
	<view>
		<block v-for="(item, index) in sightsList" :key="index">
			<view class="article-shadow tn-margin">
				<view class="tn-flex">
					<view class="tn-margin-sm tn-padding-top-xs" style="width: 100%;">
						<view class="tn-text-lg tn-text-bold clamp-text-2 tn-text-justify" style="min-height: 95rpx;">
							<view style="transform: translate(0,-5rpx);"
								class="justify-content-item tn-tag-content__item tn-margin-right-xs tn-round tn-text-sm tn-text-bold"
								:class="[`tn-bg-${item.color}--light tn-color-${item.color}`]">
								<text class="tn-tag-content__item--prefix">#</text>
								{{ item.type }}
							</view>
							<text class="" @click="goSightsPages(item.id)">{{ item.name }}</text>
						</view>
						<view class="tn-padding-top-xs">
							<text class=" tn-text-sm tn-color-gray clamp-text-1">
								{{ item.desc }}
							</text>
						</view>
						<view class="tn-flex tn-flex-row-between tn-flex-col-between">
							<view class="justify-content-item tn-color-gray tn-text-center tn-color-gray--disabled"
								style="padding-top: 15rpx;">
								<text class="tn-icon-footprint tn-padding-right-xs"></text>
								<text class="tn-padding-right">{{ item.click }}</text>
								<text class="tn-icon-like-lack tn-padding-right-xs"></text>
								<text class="">{{ item.likeCount }}</text>
							</view>
						</view>
					</view>
					<view class="image-pic tn-margin-sm" :style="'background-image:url(' + item.image + ')'">
						<view class="image-article">
						</view>
					</view>
				</view>
			</view>
		</block>
		<tn-modal v-model="show" :title="title" :content="content" :button="button" @click="clickBtn"></tn-modal>
		<tn-toast ref="toast"></tn-toast>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				mu: "",
				del_id: "",
				title: '提示信息',
				content: '确定删除所选内容吗？',
				button: [{
						text: '取消',
						plain: true,
						shape: 'round'
					},
					{
						text: '确定',
						backgroundColor: 'tn-bg-indigo',
						fontColor: '#FFFFFF'
					}
				],
				total: null, //总共多少条数据
				formData: {
					pageSize: 6, //每页10条数据
					page: 1, //第几页
				},
				list: "",
				sightsList: "",
				show: false,

			}
		},
		onReachBottom() {
			let that = this
			let allTotal = this.formData.page * this.formData.pageSize
			if (allTotal < this.total) {
				//当前条数小于总条数 则增加请求页数
				that.$data.formData.page++;
				this.getData() //调用加载数据方法
			} else {
				// console.log('已加载全部数据')
			}
		},
		onLoad: function(e) {
			this.$data.mu = e.mu;
		},
		onShow() {
			this.getDate();
		},
		methods: {
			getDate() {
				this.$data.formData.page = 1;
				let that = this;
				uni.request({
					url: 0 > that.mu ? 'http://www.rural.abc/agricultural/me' :
						'http://www.rural.abc/agricultural/more', //仅为示例，并非真实接口地址。
					method: 'POST',
					data: {
						start: that.$data.formData.page,
						pageSize: that.$data.formData.pageSize,

					},
					header: {
						'token': wx.getStorageSync('token'), //自定义请求头信息
						'content-type': "application/x-www-form-urlencoded"
					},
					success: function(res) {
						console.log(res)
						that.$data.sightsList = res.data
					}
				});
				uni.request({
					url: 0 > that.mu ? 'http://www.rural.abc/agricultural/meCunt' :
						'http://www.rural.abc/agricultural/count',
					method: 'POST',
					header: {
						'token': wx.getStorageSync('token'), //自定义请求头信息
						'content-type': "application/x-www-form-urlencoded"
					},
					success: function(res) {
						console.log(res)
						that.$data.total = res.data
					}
				});
			},
			clickBtn(event) {

				let that = this
				this.show = false;
				let del_id = this.del_id;
				if (event.index == 1) {
					uni.request({
						url: "http://www.rural.abc/agricultural/delete",
						method: 'POST',
						header: {
							'token': wx.getStorageSync('token'), //自定义请求头信息
							'content-type': "application/x-www-form-urlencoded"
						},
						data: {
							id: del_id
						},
						success: function(res) {
							if (res.data == 1) {
								that.$refs.toast.show({
									title: '删除成功',
									content: '',
									icon: 'success',
									image: '',
									duration: 1500
								})
								that.getDate();
							} else {
								that.$refs.toast.show({
									title: '删除失败',
									content: '请稍后再试',
									icon: 'close-circle',
									image: '',
									duration: 1500
								})
							}
						}
					})
				}

			},
			goSightsPages(id) {
				this.show = true;
				this.del_id = id

			},
			getData() {
				let that = this
				uni.request({
					url: 0 > that.mu ? 'http://www.rural.abc/agricultural/me' :
						'http://www.rural.abc/agricultural/more',
					method: 'POST',
					data: {
						start: that.$data.formData.page,
						pageSize: that.$data.formData.pageSize,

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
							that.$data.sightsList.push(...newlist)
						}
					}
				})

			}
		}
	}
</script>

<style lang="scss" scoped>
	@import '@/static/css/templatePage/custom_nav_bar.scss';

	/* 自定义导航栏内容 start */
	.custom-nav {
		height: 100%;

		&__back {
			margin: auto 5rpx;
			font-size: 40rpx;
			margin-right: 10rpx;
			margin-left: 30rpx;
			flex-basis: 5%;
		}

		&__search {
			flex-basis: 60%;
			width: 100%;
			height: 100%;

			&__box {
				width: 100%;
				height: 70%;
				padding: 10rpx 0;
				margin: 0 30rpx;
				border-radius: 60rpx 60rpx 0 60rpx;
				font-size: 24rpx;
			}

			&__icon {
				padding-right: 10rpx;
				margin-left: 20rpx;
				font-size: 30rpx;
			}

			&__text {
				color: #AAAAAA;
			}
		}
	}

	.logo-image {
		width: 60rpx;
		height: 60rpx;
		position: relative;
		margin-top: -15rpx;
	}

	.logo-pic {
		background-size: cover;
		background-repeat: no-repeat;
		// background-attachment:fixed;
		background-position: top;
		border-radius: 50%;
	}

	/* 自定义导航栏内容 end */

	/* 资讯主图 start*/
	.image-article {
		border-radius: 8rpx;
		border: 1rpx solid #F8F7F8;
		width: 250rpx;
		height: 200rpx;
		position: relative;
	}

	.image-pic {
		background-size: cover;
		background-repeat: no-repeat;
		// background-attachment:fixed;
		background-position: top;
		border-radius: 10rpx;
	}

	.article-shadow {
		border-radius: 15rpx;
		box-shadow: 0rpx 0rpx 50rpx 0rpx rgba(0, 0, 0, 0.07);
	}

	/* 文字截取*/
	.clamp-text-1 {
		-webkit-line-clamp: 1;
		display: -webkit-box;
		-webkit-box-orient: vertical;
		text-overflow: ellipsis;
		overflow: hidden;
	}

	.clamp-text-2 {
		-webkit-line-clamp: 2;
		display: -webkit-box;
		-webkit-box-orient: vertical;
		text-overflow: ellipsis;
		overflow: hidden;
	}

	/* 标签内容 start*/
	.tn-tag-content {
		&__item {
			display: inline-block;
			line-height: 35rpx;
			padding: 5rpx 25rpx;

			&--prefix {
				padding-right: 10rpx;
			}
		}
	}

	/* 标签内容 end*/

	/* 底部tabbar start*/
	.footerfixed {
		position: fixed;
		width: 100%;
		bottom: 0;
		z-index: 999;
		background-color: #FFFFFF;
		box-shadow: 0rpx 0rpx 30rpx 0rpx rgba(0, 0, 0, 0.07);
	}

	.tabbar {
		display: flex;
		align-items: center;
		min-height: 110rpx;
		justify-content: space-between;
		padding: 0;
		height: calc(110rpx + env(safe-area-inset-bottom) / 2);
		padding-bottom: calc(env(safe-area-inset-bottom) / 2);
	}

	.tabbar .action {
		font-size: 22rpx;
		position: relative;
		flex: 1;
		text-align: center;
		padding: 0;
		display: block;
		height: auto;
		line-height: 1;
		margin: 0;
		overflow: initial;
	}

	.bar-center {
		animation: suspension 3s ease-in-out infinite;
	}

	@keyframes suspension {

		0%,
		100% {
			transform: translateY(0);
		}

		50% {
			transform: translateY(-0.8rem);
		}
	}

	.tabbar .action .bar-icon {
		width: 100rpx;
		position: relative;
		display: block;
		height: auto;
		margin: 0 auto 10rpx;
		text-align: center;
		font-size: 42rpx;
		// line-height: 50rpx;
	}

	.tabbar .action .bar-icon image {
		width: 50rpx;
		height: 50rpx;
		display: inline-block;
	}

	.tabbar .action .bar-circle {
		position: relative;
		display: block;
		margin: 0rpx auto 0rpx;
		text-align: center;
		font-size: 60rpx;
		line-height: 100rpx;
		background-color: #FBBF18;
		width: 100rpx !important;
		height: 100rpx !important;
		overflow: hidden;
		border-radius: 50%;
		box-shadow: 0px 10px 30px rgba(70, 23, 129, 0.12),
			0px -8px 40px rgba(255, 255, 255, 1),
			inset 0px -10px 10px rgba(70, 23, 129, 0.05),
			inset 0px 10px 20px rgba(255, 255, 255, 1);
		box-shadow: 0rpx 0rpx 20rpx 0rpx rgba(251, 191, 24, 0.5);
	}

	.tabbar .action .bar-circle image {
		width: 100rpx;
		height: 100rpx;
		display: inline-block;
		margin: 0rpx auto 0rpx;
	}

	/* 流星+悬浮 */
	.nav-index-button {
		animation: suspension 3s ease-in-out infinite;
		z-index: 999999;


		&__content {
			position: absolute;
			width: 100rpx;
			height: 100rpx;
			top: 50%;
			left: 50%;
			transform: translate(-50%, -50%);

			&--icon {
				width: 100rpx;
				height: 100rpx;
				font-size: 60rpx;
				border-radius: 50%;
				margin-bottom: 18rpx;
				position: relative;
				z-index: 1;
				transform: scale(0.85);

				&::after {
					content: " ";
					position: absolute;
					z-index: -1;
					width: 100%;
					height: 100%;
					left: 0;
					bottom: 0;
					border-radius: inherit;
					opacity: 1;
					transform: scale(1, 1);
					background-size: 100% 100%;
					// background-image: url(https://tnuiimage.tnkjapp.com/cool_bg_image/icon_bg6.png);
				}
			}
		}

		&__meteor {
			position: absolute;
			top: 50%;
			left: 50%;
			width: 100rpx;
			height: 100rpx;
			transform-style: preserve-3d;
			transform: translate(-50%, -50%) rotateY(75deg) rotateZ(10deg);

			&__wrapper {
				width: 100rpx;
				height: 100rpx;
				transform-style: preserve-3d;
				animation: spin 20s linear infinite;
			}

			&__item {
				position: absolute;
				width: 100rpx;
				height: 100rpx;
				border-radius: 1000rpx;
				left: 0;
				top: 0;

				&--pic {
					display: block;
					width: 100%;
					height: 100%;
					background: url(https://tnuiimage.tnkjapp.com/cool_bg_image/arc1.png) no-repeat center center;
					background-size: 100% 100%;
					animation: arc 4s linear infinite;
				}
			}
		}
	}


	@keyframes suspension {

		0%,
		100% {
			transform: translateY(0);
		}

		50% {
			transform: translateY(-0.6rem);
		}
	}

	@keyframes spin {
		0% {
			transform: rotateX(0deg);
		}

		100% {
			transform: rotateX(-360deg);
		}
	}

	@keyframes arc {
		to {
			transform: rotate(360deg);
		}
	}
</style>