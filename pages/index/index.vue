<template>
	<view class="app-container">
		<my-header title="首页"></my-header>
		<view class="exclude-layout-container">
			<longList :items="dataList" :minNum="20" :prepareViewNum="10" scrollViewHeight="calc(100vh -  220rpx)"
				:itemViewHeight="itemHeight" :visibleViewNum="10" :loadingStatus="loadingStatus"
				@scrollBottom="getList">
				<template v-slot:default="slotItem">
					<view style="height: 80upx;border: 3rpx solid #000000;box-sizing: border-box; display: block;">
						{{ slotItem.item ? slotItem.item.value : '空空' }}
					</view>
				</template>
			</longList>
		</view>
		<my-footer></my-footer>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				dataList: [],
				itemHeight: 0,
				loadingStatus: 'more'
			}
		},
		methods: {
			goLastPage() {

			},
			getList() {
				console.log(111111111)
				this.loadingStatus = 'loading';
				if (this.dataList.length >= 200) {
					this.loadingStatus = 'nomore';
					return;
				}
				const items = []
				// // 添加测试数据，模拟分页
				for (let i = this.dataList.length; i < this.dataList.length + 30; i++) {
					items.push({
						id: i,
						value: `item${i}`
					});
				}
				setTimeout(() => {
					this.dataList = [...this.dataList, ...items]
					this.loadingStatus = 'more';
				}, 1000)
			}
		},
		onReady() {
			const items = [];
			for (let i = 0; i < 30; i++) {
				items[i] = {
					id: i,
					value: `item${i}`
				};
			}
			this.dataList = items;
			this.itemHeight = +uni.upx2px(80);
		}
	}
</script>

<style lang="scss">

</style>
