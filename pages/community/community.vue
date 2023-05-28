<template>
	<view class="tn-bg-white" style="margin-top: 0rpx;border-radius: 50rpx 50rpx 0 0;">
		
		<view class="tn-flex tn-margin-xs tn-padding-top-xl">

			<view class="tn-flex-1 screen-shadow" style="margin: 30rpx 20rpx;padding: 60rpx 0;">



				<view class="tn-flex tn-flex-direction-column tn-flex-row-center tn-flex-col-center" @click="goto1()">
					<view class="icon7__item--icon tn-flex tn-flex-row-center tn-flex-col-center">
						<image class="" src='https://tnuiimage.tnkjapp.com/icon/5.jpg' mode='aspectFit'
							></image>
					</view>
					<view class="tn-text-center">
						<view class="tn-text-ellipsis tn-color-black tn-text-lg ">乡村景点</view>
						<view class="tn-text-ellipsis tn-color-gray--dark">乡村景点</view>
					</view>
				</view>
			</view>
			<view class="tn-flex-1 screen-shadow" style="margin: 30rpx 20rpx;padding: 60rpx 0;">
				<view class="tn-flex tn-flex-direction-column tn-flex-row-center tn-flex-col-center" @click="goto2()">
					<view class="icon7__item--icon tn-flex tn-flex-row-center tn-flex-col-center">
						<image class="" src='https://tnuiimage.tnkjapp.com/icon/6.jpg' mode='aspectFit'
							></image>
					</view>
					<view class="tn-text-center">
						<view class="tn-text-ellipsis tn-color-black tn-text-lg ">农业活动</view>
						<view class="tn-text-ellipsis tn-color-gray--dark">农业活动</view>
					</view>
				</view>
			</view>
		</view>

		<view class="tn-flex tn-margin-xs" style="margin-top: -10rpx;">
			<view class="tn-flex-1  screen-shadow" style="margin: 10rpx 20rpx;padding: 60rpx 0;">
				<view class="tn-flex tn-flex-direction-column tn-flex-row-center tn-flex-col-center" @click="goto3()">
					<view class="icon7__item--icon tn-flex tn-flex-row-center tn-flex-col-center">
						<image class="" src='https://tnuiimage.tnkjapp.com/icon/7.jpg' mode='aspectFit'
							></image>
					</view>
					<view class="tn-text-center">
						<view class="tn-text-ellipsis tn-color-black tn-text-lg">乡村动态</view>
						<view class="tn-text-ellipsis tn-color-gray--dark">乡村动态</view>
					</view>
				</view>
			</view>
			<view class="tn-flex-1  screen-shadow" style="margin: 10rpx 20rpx;padding: 60rpx 0;">
				<view class="tn-flex tn-flex-direction-column tn-flex-row-center tn-flex-col-center" @click="goto4()">
					<view class="icon7__item--icon tn-flex tn-flex-row-center tn-flex-col-center">
						<image class="" src='https://tnuiimage.tnkjapp.com/icon/8.jpg' mode='aspectFit'
							></image>
					</view>
					<view class="tn-text-center" >
						<view class="tn-text-ellipsis tn-color-black tn-text-lg " >乡村就业</view>
						<view class="tn-text-ellipsis tn-color-gray--dark">乡村就业</view>
					</view>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {

			}
		},
		methods: {
			goto1(e) {
				
				uni.navigateTo({
					url: '/pages/new_sights/new_sights'
				});
			},
			goto2(e) {
				uni.navigateTo({
					url: '/pages/new_agri/new_agri'
				});
			},
			goto3(e) {
				uni.navigateTo({
					url: '/pages/new_dyna/new_dyna'
				});
			},
			goto4(e) {
				uni.navigateTo({
					url: '/pages/new_emplo/new_emplo'
				});
			},
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
				border-radius: 60rpx 60rpx 60rpx 0;
				font-size: 24rpx;
				background-color: rgba(255, 255, 255, 0.1);
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

	.screen-shadow {
		box-shadow: 0rpx 0rpx 80rpx 0rpx rgba(0, 0, 0, 0.07);
		border-radius: 20rpx;
	}

	/* 图标容器15 start */
	.icon15 {
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
				width: 100rpx;
				height: 100rpx;
				font-size: 60rpx;
				border-radius: 50%;
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


				}
			}
		}
	}

	/* 图标容器7 start */
	.icon7 {
		&__item {
			width: 30%;
			background-color: #FFFFFF;
			border-radius: 10rpx;
			padding: 10rpx;
			margin: 20rpx 10rpx;
			transform: scale(1);
			transition: transform 0.3s linear;
			transform-origin: center center;

			&--icon {
				width: 100rpx;
				height: 100rpx;
				font-size: 60rpx;
				border-radius: 0;
				margin-bottom: 18rpx;
				position: relative;
				z-index: 1;
			}
		}
	}

	/* 文章内容 start*/
	.tn-blogger-content {
		&__wrap {
			box-shadow: 0rpx 0rpx 80rpx 0rpx rgba(0, 0, 0, 0.09);
			border-radius: 20rpx;
			margin: 30rpx;
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
			border-radius: 16rpx 16rpx 0 0;

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

	.image-screen {
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

	/* 移动背景部分 start*/
	.bg-contaniner {
		position: fixed;
		top: -0rpx;
		left: -300rpx;
		--text-color: hsl(0 95% 60%);
		--bg-color: hsl(0 0% 100%);
		--bg-size: 200px;
		height: 100%;
		display: grid;
		place-items: center;
		place-content: center;
		overflow: hidden;
		background-color: var(--bg-color);
		z-index: -1;
	}

	.bg-contaniner::before {
		--size: 150vmax;
		content: "";
		inline-size: var(--size);
		block-size: var(--size);
		background-image: url("https://tnuiimage.tnkjapp.com/animate/animate3.png");
		background-size: var(--bg-size);
		background-repeat: repeat;
		transform: rotate(45deg);
		opacity: 0.25;
		animation: bg 6s linear infinite;
	}

	@media (prefers-reduced-motion: reduce) {
		.bg-contaniner::before {
			animation-duration: 0s;
		}
	}

	@keyframes bg {
		to {
			background-position: 0 calc(var(--bg-size) * -1);
		}
	}

	/* 移动背景部分 end*/

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
		background-color: #FFCA28;
		width: 90rpx !important;
		height: 90rpx !important;
		overflow: hidden;
		border-radius: 50%;
		box-shadow: 0rpx 0rpx 20rpx 0rpx rgba(255, 202, 40, 0.5);
	}

	.tabbar .action .bar-circle image {
		width: 60rpx;
		height: 60rpx;
		display: inline-block;
		margin: 15rpx auto 15rpx;
	}
</style>
