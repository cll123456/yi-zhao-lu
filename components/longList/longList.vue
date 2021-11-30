<template>
	<view class="longList--constainer">
		<!-- 数据达到阈值，进行虚拟列表来进行滚动 -->
		<block v-if="items && items.length > minNum">
			<scroll-view scroll-y="true"
				:style="{ minHeight: scrollviewMinHeight + 'px', height: scrollViewHeight, position: 'relative' }"
				@scrolltolower="scrollBottom" @scroll="handleScroll">
				<view :style="{ height: layoutHeight + 'px' }">
					<view :style="{ transform: `translateY(${offset}px)` }">
						<view v-for="(item, index) in viewCount" :key="index" v-show="visibleData[index] !== undefined">
							<slot name="default" :item="visibleData[index]"></slot>
						</view>
						<uni-load-more :status="loadingStatus" :iconSize="iconSize"></uni-load-more>
					</view>
				</view>	
			</scroll-view>
		</block>
		<!-- 进行原生的普通遍历 -->
		<block v-else>
			<scroll-view scroll-y :style="{ height: scrollViewHeight + 'px' }">
				<view v-for="(item, index) in items" :key="index">
					<slot name="default" :item="item"></slot>
				</view>
				<uni-load-more :status="loadingStatus" :iconSize="iconSize"></uni-load-more>
			</scroll-view>
		</block>
	</view>
</template>

<script>
	export default {
		name: "longList",
		props: {
			// 每一个list item
			items: {
				type: Array,
				default: function() {
					return [];
				}
			},
			// 当前窗口的高度
			scrollViewHeight: {
				type: String,
				default: '100vh'
			},
			// 默认窗口的条数
			visibleViewNum: {
				type: Number,
				default: 5
			},
			// 窗口上面的数据条数
			prepareViewNum: {
				type: Number,
				default: 5
			},
			// 每一个item的高度
			itemViewHeight: {
				type: Number,
				default: 50
			},
			// 最少的条数
			minNum: {
				type: Number,
				default: 100
			},
			// 加载状态
			loadingStatus: {
				type: String,
				default: 'more',
				// validator: (val) => {
				// 	if (['more', 'loading', 'noMore'].indexOf(val) === -1) {
				// 		console.error('loadingStatus must one of more,loading ,noMore ')
				// 	}
				// 	return true
				// }
			},
			// 图标大小
			iconSize: Number,
		},
		data() {
			return {
				// 起始位置
				startPosition: 0,
				// 结束位置
				endPosition: this.visibleViewNum,
				// 偏移量
				offset: 0,
				// 视图数量
				viewCount: 0,
				// 布局高度
				layoutHeight: 0,
				// 最小滚动高度
				scrollviewMinHeight: 0
			};
		},
		watch: {
			// 赋值滚动
			visibleViewNum: {
				handler() {
					this.viewCount = this.visibleViewNum + 2 * this.prepareViewNum;
					this.scrollviewMinHeight = this.visibleViewNum * this.itemViewHeight;
				},
				immediate: true
			},
			// 赋值高度
			itemViewHeight: {
				handler() {
					if (this.items) {
						this.layoutHeight = this.items.length * this.itemViewHeight;
						this.scrollviewMinHeight = this.visibleViewNum * this.itemViewHeight;
					}
				},
				immediate: true
			}
		},
		computed: {
			// 前面的数据数量
			previousDataCount() {
				return Math.min(this.startPosition, this.prepareViewNum);
			},
			// 后面的数据数量
			nextDataCount() {
				return Math.min(this.items.length - this.endPosition, this.prepareViewNum);
			},
			// 可视区数据
			visibleData() {
				if (this.items.length < this.minNum) {
					return [];
				}
				const startPosition = this.startPosition - this.previousDataCount;
				const endPosition = this.endPosition + this.nextDataCount;
				return this.items.slice(startPosition, endPosition);
			}
		},
		methods: {
			/**
			 * 滚动处理
			 * @param {Object} e
			 */
			handleScroll(e) {
				//获取滑动位置
				const scrollTop = e.detail.scrollTop;
				// 计算开始位置 start position
				this.startPosition = Math.floor(scrollTop / this.itemViewHeight);
				// 计算结束位置 end position   开始位置所有显示view数
				this.endPosition = this.startPosition + this.visibleViewNum;
				// scrollTop % this.itemViewHeight 取余，核心思想
				this.offset = scrollTop - (scrollTop % this.itemViewHeight) - this.previousDataCount * this
					.itemViewHeight;
			},
			/**
			 * 滚动条触底
			 * @param {Object} e
			 */
			scrollBottom(e){
				// 向上分发，触底了
				console.log('是顶顶顶顶顶顶顶顶顶顶顶顶顶')
				this.$emit('scrollBottom')
			}
		}
	}
</script>

<style lang="scss">
	.longList--constainer {
		width: 100%;
		height: inherit;
	}
</style>
