<template>
	<!-- 用户训练计划列表 -->
	<view class="userTrainingPlanListView">
		<view
			class="bg-img   flex align-center flex-direction justify-end"
			style="background-image: url('https://ossweb-img.qq.com/images/lol/web201310/skin/big10006.jpg');height: 140px;"
		>
			<view class="text-white" style="width: 100%;text-align: center;">添加训练计划，可以更好地定制适合您的训练</view>
			<button @click="gotoAddPlan" class="cu-btn btn bg-blue">+ 添加训练计划</button>
		</view>

		<view class="list bg-white">
			<view class="flex text-gray align-center">
				<text class="className textov1 text-black text-bold">陈可可的女性塑性课程训练计划</text>
				<view class="label ">教练</view>
				<text>陈可可</text>
				<text class="date">2020/06/11</text>
			</view>
			<view class="numBox text-lg text-bold text-black margin-top-sm">2/12</view>
			<view class="cu-progress round sm"><view class="bg-red" :style="[{ width: loading ? 2/12*100+'%' : '' }]"></view></view>
		</view>
	</view>
</template>

<script>
export default {
	data() {
		return {
			loading: false
		};
	},
	onLoad: function() {
		setTimeout(() => {
			this.loading = true;
		}, 500);
	},
	methods:{
		gotoAddPlan(){
			uni.navigateTo({
				url:'/pages/trainingPlan/addTrainingPlan'
			})
		}
	}
};
</script>

<style lang="scss">
.userTrainingPlanListView {
	.btn {
		margin: 25rpx 0;
		height: 70rpx;
		border-radius: 35rpx;
	}
	.list {
		font-size: 24rpx;
		padding: 38rpx 30rpx;
		.className {
			display: inline-block;
			width: 50%;
		}
		.label {
			margin: 0 14rpx;
			padding: 0 14rpx;
			background-color: #efefef;
		}
		.date {
			flex: 1;
			text-align: right;
		}
	}
}
</style>
