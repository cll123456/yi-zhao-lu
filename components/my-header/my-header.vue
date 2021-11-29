<template>
	<view class="header-container">
		<view v-if="showBtn" class="icon-container" @click="goLastPage">
			<uni-icons type="left" size="20"></uni-icons>
		</view>
		<text>{{ title }}</text>
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

			};
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
					this.$router.go(-1);
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
		height: 120rpx;
		background: linear-gradient(45deg, $uni-primary, $uni-error);
		display: flex;
		align-items: center;
		justify-content: center;
		font-size: 32rpx;
		color: #fff;
		position: relative;
		user-select: none;
		width: 100%;

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
