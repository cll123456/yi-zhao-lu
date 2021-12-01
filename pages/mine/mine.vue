<template>
	<view class="app-container">
		<my-header title="个人中心"></my-header>
		<view class="exclude-layout-container" :style="{'margin-top': customBarH + 'px'}">
			234423
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
	}
</script>

<style lang="scss">

</style>
