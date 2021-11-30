<template>
	<view class="app-container">
		<my-header title="账单录入"></my-header>
		<view class="info-container">
			<view>
			  <text>客户名字</text>
			</view>
		</view>
	</view>

  </view>
</template>

<script>
export default {
  name: "add"
}
</script>

<style scoped lang="scss">
.app-container {
  background: #efeff4;
  .info-container {
    padding: 2%;
    width: 100%;
    height: calc(100vh - 120rpx);
    background: #fff;
    box-sizing: border-box;
  }
}
</style>