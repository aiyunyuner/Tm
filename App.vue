<script>
	export default {
		onLaunch: function() {
			console.log('App Launch');

			// token标志来判断
			uni.getStorage({
				key: 'token',
				success: function(res) {},
				fail: function(res) {
					uni.reLaunch({
						url: '/pages/login/login'
					});
				}
			});
		},
		onShow: function() {
			console.log('App Show')
		},
		onHide: function() {
			console.log('App Hide')
		}
	}
</script>

<style lang="scss">
	/*每个页面公共css */
	/* 注意要写在第一行，同时给style标签加入lang="scss"属性 */
	@import './tuniao-ui/index.scss';
	@import './tuniao-ui/iconfont.css';
</style>
