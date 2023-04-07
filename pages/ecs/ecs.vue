<template>
	<view>
		<view class="charts-box">
			<view style="text-align: center;">网站近七天的访问量</view>
			<qiun-data-charts type="column" :opts="opts" :chartData="chartData" />
		</view>

		<view class="charts-box">
			<view style="text-align: center;">网站时段访问情况</view>
			<qiun-data-charts type="line" :opts="opts5" :ontouch="true" :chartData="chartData8" />
		</view>
		<view class="charts-box">
			<view style="text-align: center;">模块访问量</view>
			<qiun-data-charts type="ring" :opts="opts2" :chartData="chartData2" />
		</view>

		<view class="charts-box">
			<view style="text-align: center;">乡村风景近期访问量</view>
			<qiun-data-charts type="line" :opts="opts3" :chartData="chartData3" />
		</view>

		<view class="charts-box">
			<view style="text-align: center;">乡村动态近期访问量</view>
			<qiun-data-charts type="line" :opts="opts3" :chartData="chartData4" />
		</view>
		<view class="charts-box">
			<view style="text-align: center;">农业活动近期访问量</view>
			<qiun-data-charts type="line" :opts="opts3" :chartData="chartData5" />
		</view>
		<view class="charts-box">
			<view style="text-align: center;">乡村就业近期访问量</view>
			<qiun-data-charts type="line" :opts="opts3" :chartData="chartData6" />
		</view>
		<view class="charts-box">
			<view style="text-align: center;">用户访问TOP5</view>
			<qiun-data-charts type="bar" :opts="opts4"  :chartData="chartData7" />
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				lista: [{
					num: '',
					time: ''
				}],
				listc: [{
					num: '',
					time: ''
				}],
				list4: [{
					num: '',
					time: ''
				}],
				list5: [{
					num: '',
					time: ''
				}],
				list6: [{
					num: '',
					time: ''
				}],
				list7: [{
					num: '',
					time: ''
				}],
				list8: [{
					num: '',
					time: ''
				}],
				listb: [{
					num: "",
					type: ""
				}],
				categories: "",
				data: "",
				categories2: "",
				data2: "",
				categories4: "",
				data4: "",
				categories5: "",
				data5: "",
				categories6: "",
				data6: "",
				categories7: "",
				data7: "",
				categories8: "",
				data8: "",
				chartData: {},
				chartData2: {},
				chartData3: {},
				chartData4: {},
				chartData5: {},
				chartData6: {},
				chartData7: {},
				chartData8: {},
				//您可以通过修改 config-ucharts.js 文件中下标为 ['column'] 的节点来配置全局默认参数，如都是默认参数，此处可以不传 opts 。实际应用过程中 opts 只需传入与全局默认参数中不一致的【某一个属性】即可实现同类型的图表显示不同的样式，达到页面简洁的需求。
				opts: {
					color: ["#1890FF", "#91CB74", "#FAC858", "#EE6666", "#73C0DE", "#3CA272", "#FC8452", "#9A60B4",
						"#ea7ccc"
					],
					padding: [15, 15, 0, 5],
					enableScroll: false,
					legend: {},
					xAxis: {
						disableGrid: true
					},
					yAxis: {
						data: [{
							min: 0
						}]
					},
					extra: {
						column: {
							type: "group",
							width: 30,
							activeBgColor: "#000000",
							activeBgOpacity: 0.08
						}
					}
				},
				opts2: {
					color: ["#1890FF", "#91CB74", "#FAC858", "#EE6666", "#73C0DE", "#3CA272", "#FC8452", "#9A60B4",
						"#ea7ccc"
					],
					padding: [5, 5, 5, 5],
					enableScroll: false,
					title: {
						name: "",
						fontSize: 15,
						color: "#666666"
					},
					subtitle: {
						name: "",
						fontSize: 25,
						color: "#7cb5ec"
					},
					extra: {
						pie: {
							activeOpacity: 0.5,
							activeRadius: 10,
							offsetAngle: 0,
							labelWidth: 15,
							border: false,
							borderWidth: 3,
							borderColor: "#FFFFFF"
						}
					}
				},
				opts3: {
					color: ["#1890FF", "#91CB74", "#FAC858", "#EE6666", "#73C0DE", "#3CA272", "#FC8452", "#9A60B4",
						"#ea7ccc"
					],
					padding: [15, 10, 0, 15],
					enableScroll: false,
					legend: {},
					xAxis: {
						disableGrid: true
					},
					yAxis: {
						gridType: "dash",
						dashLength: 2
					},
					extra: {
						line: {
							type: "straight",
							width: 2,
							activeType: "hollow"
						}
					}
				},

				opts4: {
					color: ["#1890FF", "#91CB74", "#FAC858", "#EE6666", "#73C0DE", "#3CA272", "#FC8452", "#9A60B4",
						"#ea7ccc"
					],
					padding: [15, 30, 0, 5],
					enableScroll: false,
					legend: {},
					xAxis: {
						boundaryGap: "justify",
						disableGrid: false,
						min: 0,
						axisLine: false,
						max: 40
					},
					yAxis: {},
					extra: {
						bar: {
							type: "group",
							width: 30,
							meterBorde: 1,
							meterFillColor: "#FFFFFF",
							activeBgColor: "#000000",
							activeBgOpacity: 0.08,
							linearType: "custom",
							barBorderCircle: true,
							seriesGap: 2,
							categoryGap: 2
						}

					}
				}

				,
				opts5: {
					color: ["#1890FF", "#91CB74", "#FAC858", "#EE6666", "#73C0DE", "#3CA272", "#FC8452", "#9A60B4",
						"#ea7ccc"
					],
					padding: [15, 10, 0, 15],
					enableScroll: true,
					legend: {},
					xAxis: {
						disableGrid: true,
						scrollShow: true,
						itemCount: 4
					},
					yAxis: {
						gridType: "dash",
						dashLength: 2
					},
					extra: {
						line: {
							type: "straight",
							width: 2,
							activeType: "hollow"
						}
					}
				}

			};
		},
		onReady() {
			this.getServerData();
			this.getServerData2();
			this.getServerData3();
			this.getServerData4();
			this.getServerData5();
			this.getServerData6();
			this.getServerData7();
			this.getServerData8();
		},
		methods: {
			getServerData() {
				let that = this
				uni.request({
					url: 'http://www.rural.abc/img/pv7', //仅为示例，并非真实接口地址。
					method: 'POST',
					header: {
						'token': wx.getStorageSync('token'), //自定义请求头信息
						'content-type': "application/x-www-form-urlencoded"
					},
					success: function(res) {
						console.log(res)
						that.$data.lista = res.data

						let categories = []
						let data = []
						for (let i in that.$data.lista) {
							data.push(that.$data.lista[i].num)
							categories.push(that.$data.lista[i].time)
						}
						that.$data.data = data
						that.$data.categories = categories
					}
				});
				//模拟从服务器获取数据时的延时
				setTimeout(() => {
					//模拟服务器返回数据，如果数据格式和标准格式不同，需自行按下面的格式拼接
					let res = {
						categories: that.$data.categories,
						series: [{
							name: "pv值",
							data: that.$data.data
						}]
					};
					this.chartData = JSON.parse(JSON.stringify(res));
				}, 500);
			},


			getServerData2() {
				//模拟从服务器获取数据时的延时
				let that = this
				uni.request({
					url: 'http://www.rural.abc/img/pieMoules', //仅为示例，并非真实接口地址。
					method: 'POST',
					header: {
						'token': wx.getStorageSync('token'), //自定义请求头信息
						'content-type': "application/x-www-form-urlencoded"
					},
					success: function(res) {
						console.log(res)
						that.$data.listb = res.data
					}
				});

				setTimeout(() => {
					//模拟服务器返回数据，如果数据格式和标准格式不同，需自行按下面的格式拼接
					let res = {
						series: [{
							data: that.$data.listb
						}]
					};
					this.chartData2 = JSON.parse(JSON.stringify(res));
				}, 500);
			},

			getServerData3() {


				let that = this
				uni.request({
					url: 'http://www.rural.abc/img/sightsPV7', //仅为示例，并非真实接口地址。
					method: 'POST',
					header: {
						'token': wx.getStorageSync('token'), //自定义请求头信息
						'content-type': "application/x-www-form-urlencoded"
					},
					success: function(res) {
						console.log(res)
						that.$data.listc = res.data

						let categories2 = []
						let data2 = []
						for (let i in that.$data.listc) {
							data2.push(that.$data.listc[i].num)
							categories2.push(that.$data.listc[i].time)
						}
						that.$data.data2 = data2
						that.$data.categories2 = categories2
					}
				});
				//模拟从服务器获取数据时的延时
				setTimeout(() => {
					//模拟服务器返回数据，如果数据格式和标准格式不同，需自行按下面的格式拼接
					let abc = {
						categories: that.$data.categories2,
						series: [{
							name: "风景pv值",
							data: that.$data.data2
						}]
					};
					console.log(abc)
					this.chartData3 = JSON.parse(JSON.stringify(abc));
				}, 500);


			},

			getServerData4() {


				let that = this
				uni.request({
					url: 'http://www.rural.abc/img/ruPV7', //仅为示例，并非真实接口地址。
					method: 'POST',
					header: {
						'token': wx.getStorageSync('token'), //自定义请求头信息
						'content-type': "application/x-www-form-urlencoded"
					},
					success: function(res) {
						console.log(res)
						that.$data.list4 = res.data

						let categories2 = []
						let data2 = []
						for (let i in that.$data.list4) {
							data2.push(that.$data.list4[i].num)
							categories2.push(that.$data.list4[i].time)
						}
						that.$data.data4 = data2
						that.$data.categories4 = categories2
					}
				});
				//模拟从服务器获取数据时的延时
				setTimeout(() => {
					//模拟服务器返回数据，如果数据格式和标准格式不同，需自行按下面的格式拼接
					let abc = {
						categories: that.$data.categories4,
						series: [{
							name: "动态pv值",
							data: that.$data.data4
						}]
					};
					console.log(abc)
					this.chartData4 = JSON.parse(JSON.stringify(abc));
				}, 500);


			},
			getServerData5() {


				let that = this
				uni.request({
					url: 'http://www.rural.abc/img/agPV7', //仅为示例，并非真实接口地址。
					method: 'POST',
					header: {
						'token': wx.getStorageSync('token'), //自定义请求头信息
						'content-type': "application/x-www-form-urlencoded"
					},
					success: function(res) {
						console.log(res)
						that.$data.list5 = res.data

						let categories2 = []
						let data2 = []
						for (let i in that.$data.list5) {
							data2.push(that.$data.list5[i].num)
							categories2.push(that.$data.list5[i].time)
						}
						that.$data.data5 = data2
						that.$data.categories5 = categories2
					}
				});
				//模拟从服务器获取数据时的延时
				setTimeout(() => {
					//模拟服务器返回数据，如果数据格式和标准格式不同，需自行按下面的格式拼接
					let abc = {
						categories: that.$data.categories5,
						series: [{
							name: "活动pv值",
							data: that.$data.data5
						}]
					};
					console.log(abc)
					this.chartData5 = JSON.parse(JSON.stringify(abc));
				}, 500);


			},
			getServerData6() {


				let that = this
				uni.request({
					url: 'http://www.rural.abc/img/emPV7', //仅为示例，并非真实接口地址。
					method: 'POST',
					header: {
						'token': wx.getStorageSync('token'), //自定义请求头信息
						'content-type': "application/x-www-form-urlencoded"
					},
					success: function(res) {
						console.log(res)
						that.$data.list6 = res.data

						let categories2 = []
						let data2 = []
						for (let i in that.$data.list6) {
							data2.push(that.$data.list6[i].num)
							categories2.push(that.$data.list6[i].time)
						}
						that.$data.data6 = data2
						that.$data.categories6 = categories2
					}
				});
				//模拟从服务器获取数据时的延时
				setTimeout(() => {
					//模拟服务器返回数据，如果数据格式和标准格式不同，需自行按下面的格式拼接
					let abc = {
						categories: that.$data.categories6,
						series: [{
							name: "就业pv值",
							data: that.$data.data6
						}]
					};
					console.log(abc)
					this.chartData6 = JSON.parse(JSON.stringify(abc));
				}, 500);


			},


			getServerData7() {


				let that = this
				uni.request({
					url: 'http://www.rural.abc/img/userTOP5', //仅为示例，并非真实接口地址。
					method: 'POST',
					header: {
						'token': wx.getStorageSync('token'), //自定义请求头信息
						'content-type': "application/x-www-form-urlencoded"
					},
					success: function(res) {
						console.log(res)
						that.$data.list7 = res.data

						let categories2 = []
						let data2 = []
						for (let i in that.$data.list7) {
							data2.push(that.$data.list7[i].num)
							categories2.push(that.$data.list7[i].name)
						}
						that.$data.data7 = data2
						that.$data.categories7 = categories2
					}
				});
				//模拟从服务器获取数据时的延时
				setTimeout(() => {
					//模拟服务器返回数据，如果数据格式和标准格式不同，需自行按下面的格式拼接
					let abc = {
						categories: that.$data.categories7,
						series: [{
							name: "访问次数",
							data: that.$data.data7
						}]
					};
					console.log(abc)
					this.chartData7 = JSON.parse(JSON.stringify(abc));
				}, 500);


			},
			
			
			
			
			getServerData8() {
			
			
				let that = this
				uni.request({
					url: 'http://www.rural.abc/img/dayPV', //仅为示例，并非真实接口地址。
					method: 'POST',
					header: {
						'token': wx.getStorageSync('token'), //自定义请求头信息
						'content-type': "application/x-www-form-urlencoded"
					},
					success: function(res) {
						console.log(res)
						that.$data.list8 = res.data
			
						let categories2 = []
						let data2 = []
						for (let i in that.$data.list8) {
							data2.push(that.$data.list8[i].num)
							categories2.push(that.$data.list8[i].time)
						}
						that.$data.data8 = data2
						that.$data.categories8 = categories2
					}
				});
				//模拟从服务器获取数据时的延时
				setTimeout(() => {
					//模拟服务器返回数据，如果数据格式和标准格式不同，需自行按下面的格式拼接
					let abc = {
						categories: that.$data.categories8,
						series: [{
							name: "访问次数",
							data: that.$data.data8
						}]
					};
					console.log(abc)
					this.chartData8 = JSON.parse(JSON.stringify(abc));
				}, 500);
			
			
			},
		}
	};
</script>

<style scoped>
	/* 请根据实际需求修改父元素尺寸，组件自动识别宽高 */
	.charts-box {
		width: 100%;

	}
</style>