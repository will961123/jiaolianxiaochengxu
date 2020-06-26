<template>
	<!-- 模板列表 -->
	<view class="trainingTemplateView">
		<cu-custom bgColor="bg-myblack" :isBack="true">
			<block slot="content">模板列表</block>
			<block slot="right"><text @click="selectAll" style="font-size: 34rpx;margin-right: 15rpx;">全选</text></block>
		</cu-custom>
		<view v-for="(item, index) in stortList" :key="index" :class="[item.select ? 'select' : '', 'item flex align-center justify-between bg-white select']">
			<view :class="item.select ? '  text-blue' : ' '" class="flex align-center">
				<text style="font-size: 38rpx;margin-right: 120rpx;">{{ index + 1 }}</text>
				<text class=" storeName">{{ item.name }}</text>
			</view>

			<radio-group>
				<label class="radio"><radio @click="changeStore(index)" :value="'' + index" :checked="item.select" :class="item.select ? 'checked blue' : 'blue'" /></label>
			</radio-group>
		</view>
		
		<view class="btn bg-blue">
			确定
		</view>
	</view>
</template>

<script>
export default {
	data() {
		return {
			stortList: [
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
			let list = this.stortList
			for (let idx in list) {
				list[idx].select = true
			}
			this.stortList = list
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
	padding-bottom: 45px;
	.btn{
		width: 100%;
		height: 45px;
		line-height: 45px;
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
