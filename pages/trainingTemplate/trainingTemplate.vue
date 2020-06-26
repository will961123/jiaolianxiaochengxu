<template>
	<view class="trainingTemplateView">
		<view @click="gotoTemplateList" v-for="(item, index) in list" :key="index" class="item flex align-center bg-white">
			<view class="label">{{ item.type }}</view>
			<text>{{ item.name }}</text>
		</view>

		<view @click="gotoAddTemplate" class="btn cu-btn bg-blue">创建训练模板</view>
	</view>
</template>

<script>
export default {
	data() {
		return {
			list: [{ name: '模板名称', type: '个人' }, { name: '模板名称', type: '团队' }]
		};
	},
	methods: {
		gotoTemplateList() {
			uni.navigateTo({
				url: '/pages/trainingTemplate/trainingTemplateList'
			});
		},
		gotoAddTemplate() {
			uni.navigateTo({
				url: './addTrainingTemplate'
			});
		}
	}
};
</script>

<style lang="scss">
.trainingTemplateView {
	padding-bottom: 100px;
	.item {
		color: #000;
		font-size: 28rpx;
		padding: 35rpx 30rpx;
		border-bottom: 1rpx solid #ededed;
		&:last-child {
			border-bottom: none;
		}
		.label {
			border: 1rpx solid #999;
			background-color: #f6f5f5;
			font-size: 26rpx;
			line-height: 44rpx;
			padding: 0 14rpx;
			border-radius: 8rpx;
			color: #666;
			margin-right: 24rpx;
		}
	}
	.btn {
		width: 70%;
		height: 44px;
		border-radius: 14rpx;
		position: fixed;
		left: 50%;
		bottom: 40px;
		transform: translate(-50%);
	}
}
</style>
