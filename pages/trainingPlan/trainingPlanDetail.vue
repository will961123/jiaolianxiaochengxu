<template>
	<!-- 训练计划详情 -->
	<view class="trainingPlanDetail">
		<view class="bg-img text-white padding-lr" style="background-image: url('https://ossweb-img.qq.com/images/lol/web201310/skin/big10006.jpg');height: 400rpx;">
			<view class="className flex justify-between">
				<text>陈可可的女性塑性训练计划</text>
				<text @click="gotoAddPlan" class="cuIcon cuIcon-write"></text>
			</view>
			<view class="progressBox flex justify-between align-center">
				<view style="flex: 1;margin-right: 25rpx;">
					<view class="cu-progress round "><view class="bg-red" :style="[{ width: loading ? (2 / 12) * 100 + '%' : '' }]"></view></view>
				</view>
				<text>2/12</text>
			</view>
			<view class="userBox"><image src="/static/logo.png" mode="aspectFill"></image></view>
			<view class="desc">备注：</view>
		</view>

		<view class="cu-list menu-avatar">
			<view
				class="cu-item "
				:class="modalName == 'move-box-' + 0 ? 'move-cur' : ''"
				@touchstart="ListTouchStart"
				@touchmove="ListTouchMove"
				@touchend="ListTouchEnd"
				@click="gotoClassDetail"
				data-target="move-box-0"
			>
				<view style="width: 90%;left: 40rpx;" class="content flex justify-between">
					<view class="flex align-center">
						<view style="width: 80rpx;" class="num">1</view>
						<view class="name">背部力量训练</view>
					</view>
					<view class="rowBox text-gray">
						<text>执行中</text>
						<text class="cuIcon cuIcon-right"></text>
					</view>
				</view>
				<view @click.stop="del(index)" class="move">
					<!-- <view class="bg-grey">置顶</view> -->
					<view class="bg-red">删除</view>
				</view>
			</view>

			<view
				class="cu-item "
				:class="modalName == 'move-box-' + (index+1) ? 'move-cur' : ''"
				@touchstart="ListTouchStart"
				@touchmove="ListTouchMove"
				@touchend="ListTouchEnd"
				@click="gotoAddclass" 
				v-for="(item,index) in 8"
				:key="index"
				:data-target="'move-box-'+(index+1)"
			>
				<view style="width: 90%;left: 40rpx;" class="content flex justify-between">
					<view class="flex align-center">
						<view style="width: 80rpx;" class="num">{{index+2}}</view>
						<view class="name text-gray">创建新训练课</view>
					</view>
					<view class="rowBox text-gray">
						<text class="cuIcon cuIcon-right"></text>
					</view>
				</view>
				<view @click.stop="del(index)" class="move">
					<!-- <view class="bg-grey">置顶</view> -->
					<view class="bg-red">删除</view>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
export default {
	data() {
		return {
			loading: false,
			modalName: ''
		};
	},
	onLoad: function() {
		setTimeout(() => {
			this.loading = true;
		}, 500);
	},

	methods: {
		// 查看课程详情
		gotoClassDetail(){
			uni.navigateTo({
				url:'/pages/trainingPlan/classDetail'
			})
		},
		// 添加课程
		gotoAddclass(){
			uni.navigateTo({
				url:'/pages/trainingTemplate/addTrainingClass'
			})
		},
		// 添加训练计划
		gotoAddPlan(){
			uni.navigateTo({
				url:'/pages/trainingPlan/addTrainingPlan'
			})
		},
		del(idx) {
			uni.showModal({
				title: '删除训练计划',
				content: `确定删除吗?`
			});
		},
		// ListTouch触摸开始
		ListTouchStart(e) {
			this.listTouchStart = e.touches[0].pageX;
		},
		// ListTouch计算方向
		ListTouchMove(e) {
			this.isClick = false;
			this.listTouchDirection = e.touches[0].pageX - this.listTouchStart > 0 ? 'right' : 'left';
		},
		// ListTouch计算滚动
		ListTouchEnd(e) {
			console.log( e.currentTarget.dataset.target)
			if (this.listTouchDirection == 'left') {
				this.modalName = e.currentTarget.dataset.target;
			} else {
				this.modalName = null;
			}
			this.listTouchDirection = null;
		}
	}
};
</script>

<style lang="scss">
.trainingPlanDetail {
	.className {
		line-height: 80rpx;
	}
	.progressBox {
		background-color: rgba(255, 255, 255, 0.2);
		padding: 30rpx;
		border-radius: 14rpx;
	}
	.userBox {
		width: 100rpx;
		height: 100rpx;
		margin: 25rpx 0;
		& > image {
			width: 100%;
			height: 100%;
			border-radius: 50%;
		}
	}
}
</style>
