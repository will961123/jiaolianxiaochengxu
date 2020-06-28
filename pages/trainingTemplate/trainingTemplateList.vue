<template>
	<!-- 模板列表 -->
	<view class="trainingTemplateView">
		<!-- <cu-custom bgColor="bg-myblack" :isBack="true">
			<block slot="content">模板列表</block>
			<block slot="right"><text @click="selectAll" style="font-size: 34rpx;margin-right: 15rpx;">全选</text></block>
		</cu-custom> -->
		<!-- <view class="topBox"><text @click="selectAll" style="font-size: 34rpx; ">全选</text></view> -->

		<scroll-view :style="{ height: scrollViewHeight }" id="scrollView" class="bg-white scrollView" scroll-y="true">
			<view
				@click="changeStore(index)"
				v-for="(item, index) in stortList"
				:key="index"
				:class="[item.select ? 'select' : '', 'item flex align-center justify-between bg-white select']"
			>
				<view :class="item.select ? '  text-blue' : ' '" class="flex align-center">
					<text style="font-size: 38rpx;margin-right: 120rpx;">{{ index + 1 }}</text>
					<text class=" storeName">{{ item.name }}</text>
				</view>

				<radio-group>
					<label class="radio"><radio :value="'' + index" :checked="item.select" :class="item.select ? 'checked blue' : 'blue'" /></label>
				</radio-group>
			</view>
		</scroll-view>
		<view class="btn bg-blue flex">
			<view style="flex: 1;">确认</view>
			<view style="flex: 1;" @click="selectAll" class="selectAll bg-orange">全选</view>
		</view>
	</view>
</template>

<script>
export default {
	data() {
		return {
			scrollViewHeight: 'calc(100vh - 60px)',
			stortList: [
				{ name: '第一次课程', select: false },
				{ name: '第一次课程', select: false },
				{ name: '第一次课程', select: false },
				{ name: '第一次课程', select: false },
				{ name: '第一次课程', select: false },
				{ name: '第一次课程', select: false },
				{ name: '第一次课程', select: false },
				{ name: '第一次课程', select: false },
				{ name: '第一次课程', select: false },
				{ name: '第一次课程', select: false },
				{ name: '第一次课程', select: false },
				{ name: '第一次课程', select: false },
				{ name: '第一次课程', select: false },
				{ name: '第一次课程', select: false },
				{ name: '第一次课程', select: false },
			]
		};
	},
	methods: {
		selectAll() {
			let list = this.stortList;
			for (let idx in list) {
				list[idx].select = true;
			}
			this.stortList = list;
		},
		changeStore(idx) {
			let newObj = this.stortList[idx];
			newObj.select = !newObj.select;
			this.stortList.splice(idx, 1, newObj);
		}
	}
};
</script>

<style lang="scss">
.trainingTemplateView {
	padding-bottom: 65px;
	.topBox {
		padding-right: 25rpx;
		background-color: #4c515f;
		line-height: 30px;
		color: #fff;
		text-align: right;
	}
	.btn {
		width: 100%;
		height: 60px;
		line-height: 60px;
		text-align: center;
		position: fixed;
		left: 0;
		bottom: 0;
	}
	.item {
		font-size: 28rpx;
		font-weight: 700;
		padding: 0 30rpx;
		height: 48px;
		border-bottom: 1rpx solid #ededed;
		&:last-child {
			border-bottom: none;
		}
		radio {
			transform: scale(0.8);
		}
	}
}
</style>
