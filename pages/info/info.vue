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

	</view>
</template>

<script>
	export default {
		data() {
			return {
				token: '',
				sightInfor: '',
			}
		},
		//接受前面传来的参数
		onLoad: function(e) {
			let a = e.sid;
			let that = this
			console.log(e)
			uni.getStorage({
				key: 'token',
				success: function(res) {
					console.log(res.data)
					that.$data.token = res.data
				},
				fail: function(res) {
					console.log("获取token错误")
				}
			});
			const requestTask = uni.request({
				url: "http://localhost:10010/sights/getSightsById", //仅为示例，并非真实接口地址。
				method: 'POST',
				data: {
					id: a,
				},
				header: {
					'token': toString(that.$data.token), //自定义请求头信息
					'content-type': "application/x-www-form-urlencoded"
				},
				success: function(res) {
					console.log(res)
					that.$data.sightInfor = res.data
				}
			});
		},
		methods: {

		}
	}
</script>

<style>

</style>
