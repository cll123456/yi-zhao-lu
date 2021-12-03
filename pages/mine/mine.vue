<template>
	<view class="app-container">
		<my-header title="个人中心"></my-header>
		<view class="exclude-layout-container" :style="{'margin-top': customBarH + 'px'}">
			<view class="mine-avatar--container">
				<image class="imgs" mode="center"
					src="https://bjetxgzv.cdn.bspapp.com/VKCEYUGU-uni-app-doc/6acec660-4f31-11eb-a16f-5b3e54966275.jpg"
					@error="imageError"></image>
			</view>

			<!-- 列表 -->
			<uni-list>
				<!-- 消息列表 -->
				<uni-list-chat title="消息中心" :avatar-circle="true" clickable avatar="./../../static/news.png"
					note="您收到一条新的消息" time="2020-02-02 20:20" badge-positon="right" badge-text="99"></uni-list-chat>

				<uni-list-item :show-extra-icon="true" link to="/pages/mine/updateInfo/updateInfo"
					:extra-icon="{color: '#a809e0',size: '22',type: 'settings'}" title="修改个人信息"></uni-list-item>
				<uni-list-item :show-extra-icon="true" link to="/pages/vue/index/index"
					:extra-icon="{color: '#a809e0',size: '22',type: 'contact'}" title="反馈信息"></uni-list-item>
			</uni-list>

			<!-- 开发者信息 -->
			<view class="dev-info--container">
				<view class="inline">本产品由中科软股份有限公司提供</view>
				<view class="inline">联系邮箱： 6254321abc@gmail.com</view>
			</view>
		</view>
		<my-footer></my-footer>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				customBarH: 0
			};
		},
		created() {
			const app = getApp()
			this.customBarH = app.globalData.customBarH;
		},
		methods: {
			imageError: function(e) {
				console.error('image发生error事件，携带值为' + e.detail.errMsg)
			},
			gotoNewPage(e, type){
				console.log(e,'-------',type);
			}
		}
	}
</script>

<style lang="scss" scoped>
	.exclude-layout-container {
		position: relative;


		.mine-avatar--container {
			width: 100%;
			height: 400rpx;
			line-height: 400rpx;
			text-align: center;
			background-color: #eeeeee;

			.imgs {
				width: 300rpx;
				height: 300rpx;
				margin-top: 50rpx;
				background-position: cover;
				border-radius: 50%;
			}


		}

		.dev-info--container {
			position: absolute;
			bottom: 30rpx;
			color: #bdbaba;
			text-align: center;
			width: 100%;
			font-size: 28rpx;
		}

	}
</style>
