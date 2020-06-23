<template>
	<!-- 添加训练模板 -->
	<view class="addTrainingTemplateView">
		<view class="iptBox flex justify-between align-center">
			<view class="name">模板名称</view>
			<input type="text" value="" placeholder="请输入模板名称" />
		</view>
		<view class="iptBox flex justify-between align-center">
			<view class="name">课程类型</view>
			<input type="text" value="" placeholder="请输入模板名称" />
		</view>
		<view class="iptBox flex justify-between align-center">
			<text class="name">团队共享</text>
			<picker @change="bindPickerChange" :range="groupTypeList">
				<text style="line-height:60px ;" class="text-black">{{ groupTypeList[formData.groupType] }}</text>
				<text class="cuIcon  cuIcon-right text-gray"></text>
			</picker>
		</view>
		<view class="iptBox padding-xs">
			<text class="name">模板说明</text>
			<input type="text" style="text-align: left;" value="" placeholder="请输入模板说明" />
		</view>

		<view @click="gotoAddTrainingClass" class="btn">
			<text class="cuIcon cuIcon-roundaddfill text-blue margin-right-xs"></text>
			添加训练课
		</view>
	</view>
</template>

<script>
export default {
	data() {
		return {
			groupTypeList: ['不共享', '团队共享', '连锁店共享'],
			formData: {
				groupType: 0
			}
		};
	},
	methods: {
		gotoAddTrainingClass(){
			uni.navigateTo({
				url:"/pages/trainingTemplate/addTrainingClass"
			})
		},
		bindPickerChange(e) {
			formData.groupType = e.detail.value;
		}
	}
};
</script>

<style lang="scss">
.addTrainingTemplateView {
	.iptBox {
		background-color: #fff;
		padding: 0 30rpx;
		line-height: 44px;
		border-bottom: 1rpx solid #ededed;
		&.padding-xs {
			padding-bottom: 18rpx;
		}
		&:last-child {
			border-bottom: none;
		}
		.name {
			color: #000;
			font-weight: 28rpx;
		}
		input {
			height: 44rpx;
			flex: 1;
			text-align: right;
			font-size: 28rpx;
		}
	}
	
	.btn{
		line-height: 50px;
		text-align: center;
		background-color: #fff;
		margin-top: 14px;
		font-size: 32rpx;
	}
}
</style>
