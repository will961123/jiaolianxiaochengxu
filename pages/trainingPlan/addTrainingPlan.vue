<template>
	<!-- 添加用户计划 -->
	<view class="addTrainingPlanView">
		<view class="title flex justify-center align-center">
			<input type="text" placeholder="请输入" value="陈可可的训练计划" />
			<text class="cuIcon cuIcon-write"></text>
		</view>
		<view class="item flex align-center">
			<view class="name">课程类型*</view>
			<picker @change="bindPickerChange($event, 1)" :range="courseTypeList">
				<text v-if="courseType < 0" class=" text-gray">请选择课程类型</text>
				<text v-else style="line-height:60px ;" class="text-black">{{ courseTypeList[courseType] }}</text>
			</picker>
		</view>
		<view class="item flex align-center">
			<view class="name">总课时*</view>
			<picker @change="bindPickerChange($event, 2)" :range="classHourList">
				<text v-if="classHour < 0" class=" text-gray">请选择课时</text>
				<text v-else style="line-height:60px ;" class="text-black">{{ classHourList[classHour] }}</text>
			</picker>
		</view>
		<view class="item flex align-center">
			<view class="name">课单价</view>
			<input type="digit" v-model="classMoney" value="" placeholder="调整课单价" />
		</view>
		<view class="textAreaBox">
			<view class="tit text-lg  ">
				备注
			</view>
			<textarea value="" v-model="desc" placeholder="请输入备注" />
		</view>
		
		<view @click="gotoDetail"  class="btn bg-blue">
			完成
		</view>
	</view>
</template>

<script>
export default {
	data() {
		return {
			courseTypeList: ['课程类型1', '课程类型2', '课程类型3', '课程类型4'],
			courseType: -1,
			classHourList: [1, 2, 3, 4, 5, 6],
			classHour: -1,
			classMoney: '',
			desc: ''
		};
	},
	methods: {
		gotoDetail(){
			uni.navigateTo({
				url:'/pages/trainingPlan/trainingPlanDetail'
			})
		},
		bindPickerChange(e, type) {
			switch (type) {
				case 1:
					this.courseType = e.detail.value;
					break;
				case 2:
					this.classHour = e.detail.value;
					break;
			}
		}
	}
};
</script>

<style lang="scss">
page {
	background-color: #fff;
}
.addTrainingPlanView {
	.title {
		padding: 30rpx 40rpx;
		input {
			font-size: 34rpx;
			color: #000;
			font-weight: 700;
			width: 8em;
			text-align: center;
			margin-right: 10rpx;
		}
		text {
			font-size: 38rpx;
		}
	}
	.item {
		font-size: 28rpx;
		padding: 0 30rpx;
		height: 50px;
		border-bottom: 1rpx solid #ededed;
		picker,
		input {
			font-size: 28rpx;
			flex: 1;
			text-align: right;
		}
	}
	.textAreaBox{
		padding: 0 30rpx;
		.tit{
			line-height: 100rpx;
		}
		textarea{
			height: 120px;
			font-size: 28rpx;
			width: 100%;
			padding: 10rpx 44rpx;
			background-color:#f6f6f6 ;
		}
	}
	.btn{
		text-align: center;
		line-height: 40px;
		border-radius: 8px;
		width: 85%;
		position: fixed;
		bottom: 80px;
		left: 50%;
		transform: translate(-50%);
	}
}
</style>
