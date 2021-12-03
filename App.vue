<script>
	
	export default {
		globalData: {
			// 全局设置状态栏和导航栏高度
			statusBarH: 0,
			customBarH: 0,
		},
		onLaunch: function() {
			const e = uni.getSystemInfoSync();
			// 获取手机状态栏高度
			let statusBar = e.statusBarHeight
			let customBar
			
			
			// 安卓
			// #ifndef MP
			customBar = statusBar + (e.platform == 'android' ? 50 : 45)
			// #endif
			
			// 微信小程序
			// #ifdef MP-WEIXIN 
			// 获取胶囊按钮的布局位置信息
			
			let menu = wx.getMenuButtonBoundingClientRect();
			
			// 导航栏高度 = 胶囊下距离 + 胶囊上距离 - 状态栏高度
			customBar = menu.bottom + menu.top - statusBar
			// #endif
			
			// 支付宝小程序
			// #ifdef MP-ALIPAY
			customBar = statusBar + e.titleBarHeight;
			
			// #endif
			
			// 支持nvue页面写法（兼容H5/小程序/APP/APP-Nvue）
			this.globalData.statusBarH = statusBar
			this.globalData.customBarH = customBar
		},
		onShow: function() {},
		onHide: function() {}
	}
</script>

<style lang="scss">
	/*每个页面公共css */
	@import '@/uni_modules/uni-scss';
	
	/* #ifndef APP-NVUE */
	@import '@/static/customicons.css';


	// 设置整个项目的背景色
	page {
		background-color: #f5f5f5;
	}

	/* #endif */
	.example-info {
		font-size: 14px;
		color: #333;
		padding: 10px;
	}
</style>
