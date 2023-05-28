<template>
	<view class="template-job tn-safe-area-inset-bottom">

		<!-- banner -->
		<view >
			<tn-swiper :list="banner" :height="350" :effect3d="true" mode="round" @click="toInfo"></tn-swiper>
		</view>

		<!-- 数据信息 -->
		<view class="tn-info__container tn-flex tn-flex-wrap tn-flex-col-center tn-flex-row-between tn-margin">
			<block v-for="(item, index) in tuniaoData" :key="index">
				<view
					class="tn-info__item tn-flex tn-flex-direction-row tn-flex-col-center tn-flex-row-between job-shadow">
					<view class="tn-info__item__left tn-flex tn-flex-direction-row tn-flex-col-center tn-flex-row-left">
						<view
							class="tn-info__item__left--icon tn-flex tn-flex-col-center tn-flex-row-center tn-color-white"
							:class="[`tn-bg-${item.color}`]">
							<view :class="[`tn-icon-${item.icon}`]"></view>
						</view>
						<view class="tn-info__item__left__content">
							<view class="tn-info__item__left__content--title">{{ item.title }}</view>
							<view class="tn-info__item__left__content--data tn-padding-top-xs">{{ item.value }}</view>
						</view>
					</view>
					<view class="tn-info__item__right">
						<view class="tn-info__item__right--icon">
							<view class="tn-icon-right" @click="goPage(item.url)"></view>
						</view>
					</view>
				</view>
			</block>
		</view>

		<!-- 方式16 start-->
		<!-- <view class="tn-flex tn-flex-wrap tn-margin job-shadow">
			<block v-for="(item, index) in icons" :key="index">
				
			</block>
		</view> -->
		<view class="tn-flex tn-flex-direction-column">

			<block v-for="(item,index) in BestList" :key="index">
				<view class="tn-blogger-content__wrap">
					<view class="tn-padding-top-xs">
						<!-- 方式一 -->
						<view class="tn-shadow-blur image-pic" :style="'background-image:url(' + item.image + ')'" @click="toInfo2(index)">
							<view class="image-design">
							</view>
						</view>
						<!-- 方式二 -->
						<!-- <image 
		          class="tn-blogger-content__main-image tn-shadow tn-blogger-content__main-image--1 tn-margin-bottom-sm"
		          :src="item.mainImage"
		          mode="aspectFill"
		        ></image> -->
					</view>
					<view class="tn-blogger-content__label tn-text-justify tn-margin-top tn-margin-bottom-sm">
						<text class="tn-blogger-content__label__desc tn-text-bold tn-text-xl">{{ item.name }}</text>
					</view>

					<view class="tn-flex tn-flex-row-between tn-flex-col-center tn-margin-top-xs">
						<view class="justify-content-item tn-flex tn-flex-col-center">
							<view style="margin-right: 10rpx;margin-left: 0rpx;">
								<view class="tn-color-gray">
									<text class="tn-blogger-content__count-icon tn-icon-flower"></text>
									<text class="tn-padding-right">{{ item.collectionCount }}</text>
									<text class="tn-blogger-content__count-icon tn-icon-message"></text>
									<text class="tn-padding-right">{{ item.commentCount }}</text>
									<text class="tn-blogger-content__count-icon tn-icon-like"></text>
									<text class="">{{ item.likeCount }}</text>
								</view>
							</view>
						</view>
						<view class="justify-content-item tn-text-center">
							<view v-for="(label_item,label_index) in item.label" :key="label_index"
								class="tn-blogger-content__label__item tn-float-left tn-margin-right tn-bg-gray--light tn-round tn-text-sm tn-text-bold">
								<text class="tn-blogger-content__label__item--prefix">#</text> {{ label_item }}
							</view>

						</view>
					</view>
				</view>

				<!-- 边距间隔 -->
				<view class="tn-strip-bottom" v-if="index != BestList.length - 1"></view>
			</block>
		</view>



		<!-- 方式16 end-->
	</view>
</template>

<script>
	import template_page_mixin from '@/libs/mixin/template_page_mixin.js'
	import NavIndexButton from '@/libs/components/nav-index-button.vue'
	export default {
		name: 'TemplateJob',
		mixins: [template_page_mixin],
		components: {
			NavIndexButton
		},
		data() {
			return {
				lis: [{
					image: ""
				}],
				tabbarList: [{
						title: '首页',
						activeIcon: 'home-fill',
						inactiveIcon: 'home'
					},


					{
						title: '发布',
						activeIcon: 'add-square-fill',
						inactiveIcon: 'add-square',
						count: 100,
						iconSize: 50,
						out: true
					},
					{
						title: '我的',
						activeIcon: 'my-fill',
						inactiveIcon: 'my',
						dot: true
					}
				],
				// tabbar当前被选中的序号
				currentIndex: 0,
				// 自定义底栏对应页面的加载情况
				tabberPageLoadFlag: [],
				banner: [],
				ListTab: [{
					url: "../index/index"
				}, {
					url: "../community/community"
				}, {
					url: "../me/me"
				}],
				tuniaoData: [{
						title: '乡村景点',
						icon: 'praise',
						color: 'purplered',
						value: '32',
						url: '/pages/sights/sights',
					},
					{
						title: '农业活动',
						icon: 'discover',
						color: 'green',
						value: '65',
						url: '/pages/agricultural/agricultural',
					},
					{
						title: '乡村动态',
						icon: 'topics',
						color: 'orange',
						value: '26',
						url: '/pages/dynamic/dynamic'
					},
					{
						title: '乡村就业',
						icon: 'honor',
						color: 'indigo',
						value: '6',
						url: '/pages/employment/employment'
					}
				],
				BestList: "",
			}





		},
		mounted() {
			let that = this
			uni.request({
				url: "http://www.rural.abc/sights/top3", //仅为示例，并非真实接口地址。
				method: 'POST',

				header: {
					'token': wx.getStorageSync('token'), //自定义请求头信息
					'content-type': "application/x-www-form-urlencoded"
				},
				success: function(res) {
					that.$data.lis = res.data
					let j = 1;
					for (let i in that.$data.lis) {
						if (j > 3) break;
						that.$data.banner.push(that.$data.lis[i].image)
						j++
					}
				}
			});
			
			uni.request({
				url: 'http://www.rural.abc/sights/getCount',
				method: 'POST',
				header: {
					'token': wx.getStorageSync('token'), //自定义请求头信息
					'content-type': "application/x-www-form-urlencoded"
				},
				success: function(res) {
					that.$data.tuniaoData[0].value = res.data
				}
			});
			uni.request({
				url: 'http://www.rural.abc/agricultural/count',
				method: 'POST',
				header: {
					'token': wx.getStorageSync('token'), //自定义请求头信息
					'content-type': "application/x-www-form-urlencoded"
				},
				success: function(res) {
					that.$data.tuniaoData[1].value = res.data
				}
			});
			uni.request({
				url: 'http://www.rural.abc/rural/count',
				method: 'POST',
				header: {
					'token': wx.getStorageSync('token'), //自定义请求头信息
					'content-type': "application/x-www-form-urlencoded"
				},
				success: function(res) {
					that.$data.tuniaoData[2].value = res.data
				}
			});
			uni.request({
				url: 'http://www.rural.abc/employment/count',
				method: 'POST',
				header: {
					'token': wx.getStorageSync('token'), //自定义请求头信息
					'content-type': "application/x-www-form-urlencoded"
				},
				success: function(res) {
					that.$data.tuniaoData[3].value = res.data
				}
			});

			uni.request({
				url: "http://www.rural.abc/sights/beast", //仅为示例，并非真实接口地址。
				method: 'POST',

				header: {
					'token': wx.getStorageSync('token'), //自定义请求头信息
					'content-type': "application/x-www-form-urlencoded"
				},
				success: function(res) {
					that.$data.BestList = res.data
					// let j = 1;
					// for (let i in that.$data.lis) {
					// 	if (j > 3) break;
					// 	that.$data.banner.push(that.$data.lis[i].image)
					// 	j++
					// }
				}
			});
		},
		methods: {
			toInfo(index) {
				let that = this
				uni.navigateTo({
					url: `/pages/info/info?sid=${that.$data.lis[index].id}`
				})
			},
			toInfo2(index) {
				let that = this
				uni.navigateTo({
					url: `/pages/info/info?sid=${that.$data.BestList[index].id}`
				})
			},
			goPage(e) {
				uni.navigateTo({
					url: e
				});
			}
		}
	}
</script>

<style lang="scss" scoped>
	@import '@/static/css/templatePage/custom_nav_bar.scss';

	.template-job {}

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
				background-color: rgba(255, 255, 255, 0.2);
			}

			&__icon {
				padding-right: 10rpx;
				margin-left: 20rpx;
				font-size: 30rpx;
			}

			&__text {}
		}
	}

	/* 自定义导航栏内容 end */

	/*logo start */
	.logo-image {
		width: 65rpx;
		height: 65rpx;
		position: relative;
	}

	.logo-pic {
		background-size: cover;
		background-repeat: no-repeat;
		// background-attachment:fixed;
		background-position: top;
		border-radius: 50%;
	}


	/* 信息展示 start */
	.tn-info {

		&__container {
			margin-top: 40rpx;
		}

		&__item {
			width: 48%;
			margin: 15rpx 0rpx;
			padding: 38rpx 28rpx;
			border-radius: 10rpx;

			&__left {

				&--icon {
					width: 80rpx;
					height: 80rpx;
					border-radius: 50%;
					font-size: 40rpx;
					margin-right: 20rpx;
					position: relative;
					z-index: 1;

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
						background-image: url(https://tnuiimage.tnkjapp.com/cool_bg_image/icon_bg3.png);
					}
				}

				&__content {
					font-size: 30rpx;

					&--data {
						margin-top: 5rpx;
						font-weight: bold;
					}
				}
			}

			&__right {
				&--icon {
					font-size: 30rpx;
					opacity: 0.5;
				}
			}
		}
	}

	.job-shadow {
		box-shadow: 0rpx 0rpx 80rpx 0rpx rgba(0, 0, 0, 0.07);
		border-radius: 20rpx;
	}

	/* 信息展示 end */

	/* 图标容器16 start */
	.tn-cool-color-icon16 {
		// background-image: -webkit-linear-gradient(135deg, #ED1C24, #FECE12);   16
		// background-image: linear-gradient(135deg, #ED1C24, #FECE12);
		-webkit-background-clip: text;
		background-clip: text;
		-webkit-text-fill-color: transparent;
		text-fill-color: transparent;
	}

	.icon16 {
		&__item {
			// width: 30%;
			background-color: #FFFFFF;
			border-radius: 10rpx;
			padding: 0rpx;
			margin: 0rpx;
			transform: scale(1);
			transition: transform 0.3s linear;
			transform-origin: center center;

			&--icon {
				width: 100rpx;
				height: 100rpx;
				font-size: 70rpx;
				border-radius: 50%;
				margin-bottom: 18rpx;
				position: relative;
				z-index: 1;
			}
		}
	}

	/* 图标容器16 end */

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
		margin: -30rpx auto 20rpx;
		text-align: center;
		font-size: 52rpx;
		line-height: 90rpx;
		background-color: #E72F8C;
		width: 90rpx !important;
		height: 90rpx !important;
		overflow: hidden;
		border-radius: 50%;
		box-shadow: 0rpx 0rpx 20rpx 0rpx rgba(231, 47, 140, 0.5);
	}

	.tabbar .action .bar-circle image {
		width: 60rpx;
		height: 60rpx;
		display: inline-block;
		margin: 15rpx auto 15rpx;
	}



	@import '@/static/css/templatePage/custom_nav_bar.scss';

	.template-design {}

	/* 图标容器10 start */
	.icon10 {
		&__item {
			width: 30%;
			background-color: #FFFFFF;
			border-radius: 10rpx;
			padding: 30rpx;
			margin: 20rpx 10rpx;
			transform: scale(1);
			transition: transform 0.3s linear;
			transform-origin: center center;

			&--icon {
				width: 84rpx;
				height: 65rpx;
				font-size: 45rpx;
				border-radius: 200rpx;
				margin-bottom: 18rpx;
				position: relative;
				z-index: 1;

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
					background-image: url(https://tnuiimage.tnkjapp.com/cool_bg_image/icon_bg6.png);
				}
			}
		}
	}

	/* 图标容器10 end */

	/* 文章内容 start*/
	.tn-blogger-content {
		&__wrap {
			padding: 30rpx;
		}

		&__info {
			&__btn {
				margin-right: -12rpx;
				opacity: 0.5;
			}
		}

		&__label {
			&__item {
				line-height: 45rpx;
				padding: 0 20rpx;
				margin: 5rpx 18rpx 0 0;

				&--prefix {
					color: #00FFC8;
					padding-right: 10rpx;
				}
			}

			&__desc {
				line-height: 55rpx;
			}
		}

		&__main-image {
			box-shadow: 0rpx 5rpx 40rpx 0rpx rgba(43, 158, 246, 0.2);
			border-radius: 16rpx;

			&--1 {
				max-width: 690rpx;
				min-width: 690rpx;
				max-height: 400rpx;
				min-height: 400rpx;
			}

			&--2 {
				max-width: 260rpx;
				max-height: 260rpx;
			}

			&--3 {
				height: 212rpx;
				width: 100%;
			}
		}

		&__count-icon {
			font-size: 40rpx;
			padding-right: 5rpx;
		}
	}

	.image-design {
		padding: 180rpx 0rpx;
		font-size: 40rpx;
		font-weight: 300;
		position: relative;
	}

	.image-pic {
		background-size: cover;
		background-repeat: no-repeat;
		// background-attachment:fixed;
		background-position: top;
		border-radius: 10rpx;
	}

	/* 文章内容 end*/

	/* 间隔线 start*/
	.tn-strip-bottom {
		width: 100%;
		border-bottom: 20rpx solid rgba(241, 241, 241, 0.3);
	}

	/* 间隔线 end*/

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

	.tabbar .action .bar-icon {
		width: 100rpx;
		position: relative;
		display: block;
		height: auto;
		margin: 0 auto 10rpx;
		text-align: center;
		font-size: 42rpx;
	}

	.tabbar .action .bar-icon image {
		width: 50rpx;
		height: 50rpx;
		display: inline-block;
	}
</style>