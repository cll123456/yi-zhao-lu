<template>
	<view class="my-container">
		<uni-row>
			<view class="demo-uni-row">
				<uni-col :span="8">
					<view class="col-container" @click="switchTabs(homePath)">
						<view class="tabbar-container"
							:style="{color: currentTabIndex ==='/' + homePath ? activedColor : ''}">
							<uni-icons type="home" size="20"
								:color=" currentTabIndex ==='/' + homePath ? activedColor : ''">
							</uni-icons>
							<view>首页</view>
						</view>
					</view>
				</uni-col>
				<uni-col :span="8">
					<view class="col-container" @click="switchTabs(addPath)">
						<view class="add-container">
							<uni-icons type="plusempty" size="40"></uni-icons>
						</view>
					</view>
				</uni-col>
				<uni-col :span="8">
					<view class="col-container" @click="switchTabs(minePath)">
						<view class="tabbar-container mine"
							:style="{color: currentTabIndex === '/' +minePath ? activedColor : ''}">
							<uni-icons type="person" size="20"
								:color="currentTabIndex ==='/' + minePath ? activedColor : ''">
							</uni-icons>
							<view>我的</view>
						</view>
					</view>
				</uni-col>
			</view>
		</uni-row>
	</view>
</template>

<script>
	export default {
		name: "my-footer",
		props: {
			// 激活颜色
			activedColor: {
				type: String,
				default: '#fff'
			},
			// 主页面路径
			homePath: {
				type: String,
				default: 'pages/index/index'
			},
			// 添加路径
			addPath: {
				type: String,
				default: 'pages/add/add'
			},
			// 我的路径
			minePath: {
				type: String,
				default: 'pages/mine/mine'
			},
		},
		created() {
			/* uniapp获取当前页面路径 (App、小程序、H5通用) */
			let pages = getCurrentPages() //获取页面栈数组
			let page = pages[pages.length - 1] //获取当前页面对象
			let route = page.route //获取当前页面路由
			this.selectRoute(route)
		},
		data() {
			return {
				// 当前路径
				currentTabIndex: this.homePath
			};
		},
		methods: {
			// 匹配当前路由页面
			selectRoute(curPath) {
				this.currentTabIndex = curPath == '/' ? this.homePath : '/' + curPath;
			},
			/**
			 * 底部跳转
			 * @param {Object} path
			 */
			switchTabs(path) {
				this.selectRoute(path);

				uni.navigateTo({
					url: '/' + path
				})
			}
		}
	}
</script>

<style lang="scss">
	@import './../../mixin.scss';

	.my-container {
		width: 100%;
		position: fixed;
		background: lighten($color: $uni-primary, $amount: 30);
		bottom: 0;
		height: 100rpx;
		// 添加底部安全距离
		// height: calc(100rpx + constant(safe-area-inset-bottom)); //兼容 IOS<11.2
		// height: calc(100rpx + env(safe-area-inset-bottom)); //兼容 IOS>11.2

		// // 如果不支持安全距离，直接使用高度固定
		// @supports not (height: calc(100rpx + constant(safe-area-inset-bottom))) {
		// 	height: 100rpx;
		// }


		.demo-uni-row {
			display: flex;
			align-items: center;
			width: 100%;
			height: 100rpx;
			justify-content: center;

			.col-container {
				// text-align: center;
				position: relative;
				width: 100%;

				.tabbar-container {
					display: flex;
					flex-direction: column;
					align-items: center;
				}

				.add-container {
					border-top: 6rpx solid #dd99ff;
					border-top-right-radius: 30rpx;
					border-top-left-radius: 30rpx;
					width: 100rpx;
					height: 100rpx;
					display: flex;
					align-items: center;
					justify-content: center;
					background-color: #eee;
					transform: translateX(-50%);
					margin-left: 50%;
				}

			}
		}


	}
</style>
