<template>
	<view class="my-container">
		<uni-row class="demo-uni-row">
			<uni-col :span="8" class="col-container">
				<view class="tabbar-container">
					<uni-icons type="home" size="20"></uni-icons>
					<view>首页</view>
				</view>
			</uni-col>
			<uni-col :span="8" class="col-container">

				<uni-icons class="add-container" type="plusempty" size="40"></uni-icons>
			</uni-col>
			<uni-col :span="8" class="col-container">
				<view class="tabbar-container mine">
					<uni-icons type="person" size="20"></uni-icons>
					<view>我的</view>
				</view>
			</uni-col>
		</uni-row>



	</view>
</template>

<script>
	export default {
		name: "my-footer",
		data() {
			return {

			};
		}
	}
</script>

<style lang="scss">
	@import './../../mixin.scss';

	.my-container {
		width: 100%;
		position: absolute;
		background: lighten($color: $uni-primary, $amount: 30);
		bottom: 0;
		// 添加底部安全距离
		height: calc(100rpx + constant(safe-area-inset-bottom)); //兼容 IOS<11.2
		height: calc(100rpx + env(safe-area-inset-bottom)); //兼容 IOS>11.2
		padding-bottom: constant(safe-area-inset-bottom); //兼容 IOS<11.2
		padding-bottom: env(safe-area-inset-bottom); //兼容 IOS>11.2

		// 如果不支持安全距离，直接使用高度固定
		@supports not (height: calc(100rpx + constant(safe-area-inset-bottom))) {
			height: 100rpx;
		}


		.demo-uni-row {
			display: flex;
			align-items: center;
			width: 100%;
			height: inherit;
			justify-content: center;

			.col-container {
				text-align: center;
				position: relative;
				width: 100%;

				.tabbar-container {
					display: flex;
					flex-direction: column;

				}

				.add-container {
					width: 100rpx;
					height: 100rpx;
					border-top: 6rpx solid #dd99ff;
					// border-radius: 6rpx;
					border-top-right-radius: 30rpx;
					border-top-left-radius: 30rpx;
					background-color: #eee;
					padding: 20rpx;
				}

			}
		}


	}
</style>
