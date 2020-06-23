<template>
	<!-- 编辑动作要领-->
	<view class="addActionPointsView">
		<view class="titBox">动作介绍</view>
		<view class="textarea bg-white"><textarea v-model="formData.ipt1" placeholder="请输入" /></view>

		<view v-show="item.select" v-for="(item, index) in pointsList" :key="index">
			<view class="titBox flex justify-between">
				<text>{{ item.name }}</text>
				<text @click="item.select = false" style="font-size: 38rpx;" class="text-gray cuIcon cuIcon-roundclosefill"></text>
			</view>
			<view class="textarea bg-white"><textarea v-model="formData['ipt' + (index + 2)]" placeholder="请输入" /></view>
		</view>

		<view class="typeBox flex">
			<view class="item" v-if="!item.select" @click="item.select = true" v-for="(item, index) in pointsList" :key="index">{{ item.name }}</view>
		</view>

		<button @click="back" class="btn cu-btn bg-blue">完成</button>
	</view>
</template>

<script>
export default {
	data() {
		return {
			formData: {
				ipt1: '',
				ipt2: '',
				ipt3: '',
				ipt4: '',
				ipt5: ''
			},
			pointsList: [{ name: '动作步骤', select: false }, { name: '呼吸', select: false }, { name: '动作感受', select: false }, { name: '常见错误', select: false }]
		};
	},
	methods: {
		back() {
			uni.navigateBack({
				delta: 1
			});
		}
	}
};
</script>

<style lang="scss">
.addActionPointsView {
	padding-bottom: 48px;
	.titBox {
		line-height: 50px;
		font-size: 32rpx;
		background-color: #f6f6f6;
		color: #666;
		padding: 0 30rpx;
	}
	.textarea {
		width: 100%;
		height: 200px;
		padding: 50rpx 30rpx;
		textarea {
			width: 100%;
			height: 100%;
		}
	}
	.typeBox {
		margin-top: 25rpx;
		margin-left: 25rpx;
		.item {
			background-color: rgba(0, 0, 0, 0.3);
			color: #fff;
			line-height: 58rpx;
			border-radius: 6rpx;
			padding: 0 22rpx;
			margin-right: 25rpx;
		}
	}
	.btn {
		border-radius: 0;
		width: 100%;
		height: 44px;
		position: fixed;
		left: 0;
		bottom: 0;
	}
}
</style>
