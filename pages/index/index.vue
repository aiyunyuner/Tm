<template>
	<view>
		<view :style="{display: currentIndex === 0 ? '' : 'none'}">
			<scroll-view>
				<index ref="index"></index>
			</scroll-view>
		</view>
		<view :style="{display: currentIndex === 1 ? '' : 'none'}">
			<scroll-view>
				<community ref="community"></community>
			</scroll-view>
		</view>
		<view :style="{display: currentIndex === 2 ? '' : 'none'}">
			<scroll-view>
				<me ref="me"></me>
			</scroll-view>
		</view>
		<tn-tabbar v-model="currentIndex" :list="tabbarList" activeColor="#838383" inactiveColor="#AAAAAA"
			activeIconColor="tn-cool-bg-color-7" :animation="true" :safeAreaInsetBottom="true" @change="switchTabbar">
		</tn-tabbar>
	</view>
</template>

<script>
	import me from "../me/me.vue"
	import index from "../index_one/index_one.vue"
	import community from "../community/community.vue"
	export default {
		components: {
			me,
			community,
			index
		},
		data() {
			return {

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
				ListTab: [{
					url: "../index/index"
				}, {
					url: "../community/community"
				}, {
					url: "../me/me"
				}],
			}
		},
		methods: {
			switchTabbar(index) {
				// if (index != this.$data.currentIndex) {
				// 	let a = this.$data.ListTab[index].url
				// 	// console.log(a)
				// 	uni.redirectTo({
				// 		url: a
				// 	});
				// }

			},

		}
	}
</script>

<style>

</style>
