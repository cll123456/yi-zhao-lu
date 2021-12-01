<template>
	<view class="header-container" :style="{'height': appData.customBarH + 'px'}">
		<view v-if="showBtn" class="icon-container" @click="goLastPage">
			<uni-icons type="left" size="20"></uni-icons>
		</view>
		<view class="tex-container" :style="{'top': appData.statusBarH + 10 + 'px'}">
			<text>{{ title }}</text>
		</view>
	</view>
</template>

<script>
	export default {
		name: "my-header",
		props: {
			// 是否显示返回上一页的按钮
			showBtn: Boolean,
			// 当前页面的名称
			title: String,
		},
		data() {
			return {
				appData:{
					statusBarH: 0,
					customBarH: 0
				}
			};
		},
		created() {
			const app = getApp()
			// 获取状态栏和导航条高度
			this.appData.statusBarH = app.globalData.statusBarH;
			
			
			this.appData.customBarH = app.globalData.customBarH;
			
			// #ifdef MP-ALIPAY
			this.appData.customBarH = app.globalData.customBarH - 20;
			// #endif
			
		},
		methods: {
			/**
			 * 自定义返回上一页按钮
			 */
			goLastPage() {
				// 向上分发
				this.$emit("goLastPage");
				// 如果当前传入了自定义事件，那么就使用自定义事件，否则直接router(-1)
				if (typeof this.$listeners.goLastPage === 'function') {
					this.$listeners.goLastPage();
				} else {
					uni.navigateBack({
						delta: 1
					})
				}
			}
		}
	}
</script>

<style lang="scss">
	@import './../../mixin.scss';
	$uni-primary: #2979ff !default;
	$uni-success: #4cd964 !default;
	$uni-warning: #f0ad4e !default;
	$uni-error: #dd524d !default;
	$uni-info: #909399 !default;


	.header-container {
		z-index: 2021;
		background: linear-gradient(45deg, $uni-primary, $uni-error);
		display: flex;
		align-items: center;
		justify-content: center;
		font-size: 32rpx;
		color: #fff;
		position: fixed;
		user-select: none;
		width: 100%;

		// 支付宝小程序需要额外些样式
		// #ifdef MP-ALIPAY
		justify-content: flex-start;
		padding-top: 30rpx;
		padding-left: 80rpx;

		.tex-container {
			position: absolute;
		}

		// #endif

		// 微信小程序的样式
		// #ifdef MP-WEIXIN
		.tex-container {
			position: absolute;
		}

		// #endif



		span {
			width: 70%;
			margin: 0 auto;
			@include overOneDot;
			text-align: center;
		}

		.icon-container {
			position: absolute;
			height: 100%;
			left: 20rpx;
			font-size: 40rpx;
			display: flex;
			align-items: center;

			&:hover,
			&:focus,
			&:active {
				color: mix(#fff, #000, 80);
			}
		}
	}
</style>
