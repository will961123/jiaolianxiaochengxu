<template>
	<!-- 训练计划详情 -->
	<view class="trainingPlanDetail">
		<!-- <cu-custom bgColor="bg-myblack" :isBack="true"> 
			<block slot="right">
				<text @click="gotoTemplate" class="addTemolate">
					<text class="cuIcon cuIcon-share"></text>
					导入模板
				</text>
			</block>
		</cu-custom> -->

		<view class="bg-img text-white padding-lr" style="background-image: url('https://ossweb-img.qq.com/images/lol/web201310/skin/big10006.jpg');height: 400rpx;">
			<view class="className flex justify-between align-center">
				<text>陈可可的女性塑性训练计划</text>
				<view>
					<text @click="gotoAddPlan" class="cuIcon cuIcon-write"></text>
					<!-- <text @click="gotoTemplate" class="addTemolate">
						<text class="cuIcon cuIcon-share"></text>
						导入模板
					</text> -->
				</view>
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
		<scroll-view :style="{ height: scrollViewHeight }" id="scrollView" class="bg-white scrollView" scroll-y="true">
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
					:class="modalName == 'move-box-' + (index + 1) ? 'move-cur' : ''"
					@touchstart="ListTouchStart"
					@touchmove="ListTouchMove"
					@touchend="ListTouchEnd"
					@click="gotoAddclass"
					v-for="(item, index) in 8"
					:key="index"
					:data-target="'move-box-' + (index + 1)"
				>
					<view style="width: 90%;left: 40rpx;" class="content flex justify-between">
						<view class="flex align-center">
							<view style="width: 80rpx;" class="num">{{ index + 2 }}</view>
							<view class="name text-gray">创建新训练课</view>
						</view>
						<view class="rowBox text-gray"><text class="cuIcon cuIcon-right"></text></view>
					</view>
					<view @click.stop="del(index)" class="move">
						<!-- <view class="bg-grey">置顶</view> -->
						<view class="bg-red">删除</view>
					</view>
				</view>
			</view>
		</scroll-view>
		
		<view  @click="gotoTemplate" class="btn bg-blue">
			导入模板
		</view>
	</view>
</template>

<script>
export default {
	data() {
		return {
			loading: false,
			modalName: '',
			// scrollViewHeight: 'calc(100vh - 260px)'
			scrollViewHeight: 'calc(100vh - 199.98px - 60px)'
		};
	},
	onLoad: function() {
		setTimeout(() => {
			this.loading = true;
		}, 500);
	},
	onReady() {
		const query = uni.createSelectorQuery().in(this);
		query
			.select('#scrollView')
			.boundingClientRect(data => {
				let h =Number( data.top.toFixed(2)) + 60
				this.scrollViewHeight = 'calc(100vh - ' +  h + 'px)'; 
				console.log(this.scrollViewHeight);
			})
			.exec();
	},
	methods: {
		gotoTemplate() {
			uni.navigateTo({
				url: '/pages/trainingTemplate/trainingTemplate'
			});
		},

		// 查看课程详情
		gotoClassDetail() {
			uni.navigateTo({
				url: '/pages/trainingPlan/classDetail'
			});
		},
		// 添加课程
		gotoAddclass() {
			uni.navigateTo({
				url: '/pages/trainingTemplate/addTrainingClass'
			});
		},
		// 添加训练计划
		gotoAddPlan() {
			uni.navigateTo({
				url: '/pages/trainingPlan/addTrainingPlan'
			});
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
			if (Math.abs(e.touches[0].pageX - this.listTouchStart) < 35) {
				return;
			}
			this.listTouchDirection = e.touches[0].pageX - this.listTouchStart > 0 ? 'right' : 'left';
		},
		// ListTouch计算滚动
		ListTouchEnd(e) {
			console.log(e.currentTarget.dataset.target);
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
	.addTemolate {
		display: inline-block;
		background-color: rgba(0, 0, 0, 0.3);
		font-size: 28rpx;
		margin-right: 14rpx;
		line-height: 40rpx;
		height: 40rpx;
		padding: 0 15rpx;
		border-radius: 20rpx;
	}

	.className {
		line-height: 80rpx;
		font-size: 34rpx;
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
	.btn{
		width: 100%;
		position: fixed;
		left: 50%;
		transform: translate(-50%);
		bottom: 0px;
		height: 60px;
		line-height: 60px;
		text-align: center; 
		margin: 0 auto;
	}
}
</style>
