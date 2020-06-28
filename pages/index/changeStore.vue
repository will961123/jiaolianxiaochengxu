<template>
	<view class="changeStoreView">
		<!-- <cu-custom bgColor="bg-myblack" :isBack="true"><block slot="content">切换团队</block></cu-custom> -->
		<view @click="changeStore(index)" v-for="(item, index) in stortList" :key="index" :class="[index === selectIdx ? 'select' : '', 'item flex align-center justify-between bg-white select']">
			<text :class="index === selectIdx ? 'storeName text-blue' : 'storeName'">{{ item.name }}</text>
			<radio-group>
				<label class="radio">
					<radio  :value="'' + index" :checked="index === selectIdx" :class="index === selectIdx ? 'checked blue' : 'blue'" />
				</label>
			</radio-group>
		</view>
	</view>
</template>

<script>
export default {
	data() {
		return {
			selectIdx: 0,
			stortList: new Array(4).fill({ name: '黄金时代未来店' })
		};
	},
	methods: {
		changeStore(idx) {
			if (idx === this.selectIdx) {
				return;
			}
			this.selectIdx = idx;
		}
	}
};
</script>

<style lang="scss">
.changeStoreView {
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
