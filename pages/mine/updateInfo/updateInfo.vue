<template>
	<view class="app-container">
		<my-header title="修改个人信息" :showBtn="true"></my-header>
		<!-- 修改个人信息表单 -->
		<view class="forms-containers" :style="{'margin-top': customBarH + 'px'}">
			<uni-forms ref="dynamicForm" :rules="dynamicRules" :modelValue="dynamicFormData" label-align="right" border>
				<uni-forms-item label="昵称" required name="email">
					<uni-easyinput v-model="dynamicFormData.email" placeholder="请输入昵称" />
				</uni-forms-item>
				<uni-forms-item label="手机号码" required name="email">
					<uni-easyinput v-model="dynamicFormData.email" disabled :styles="readonlyStyles"
						placeholderStyle="color: #999" placeholder="请输入手机号码" />
				</uni-forms-item>
 
			</uni-forms>
			<view class="button-group">
				<button class="uni-button[type=primary]" type="primary" block  @click="submit('dynamicForm')">提交</button>
			</view>
		</view>
	</view>
</template>

<script>
	import {
		readonlyStyles
	} from '../../../utils/formsCommon.js';

	export default {
		data() {
			return {
				customBarH: 0,
				// 数据源
				dynamicFormData: {
					email: '',
					domains: {}
				},
				// 输入框只读属性
				readonlyStyles: readonlyStyles,
				// 规则
				dynamicRules: {
					email: {
						rules: [{
							required: true,
							errorMessage: '域名不能为空'
						}, ]
					}
				}
			}
		},
		created() {
			const app = getApp()
			this.customBarH = app.globalData.customBarH;
		},

		methods: {
			// 新增表单域
			add() {
				this.dynamicLists.push({
					label: '域名',
					id: Date.now()
				})
			},
			// 删除表单域
			del(id) {
				let index = this.dynamicLists.findIndex(v => v.id === id)
				this.dynamicLists.splice(index, 1)
			},
			// 提交
			submit(ref) {
				this.$refs[ref].validate((err, value) => {
					console.log(err, value);
				})
			},
		}
	}
</script>

<style lang="scss">
	.forms-containers {
		background-color: #fff;
		padding: 20rpx;
	}
</style>
