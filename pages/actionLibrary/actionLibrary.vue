<template>
	<view class="actionLibraryView">
		<cu-custom bgColor="bg-myblack" :isBack="true">
			<block slot="content">动作库</block>
			<block slot="right"><text @click="gotoAddAction" style="font-size: 18px;font-weight: 700;margin-right: 14px;" class="cuIcon cuIcon-add"></text></block>
		</cu-custom>
		<view class="searchBox bg-white">
			<view class="iptBox flex align-center">
				<text class="cuIcon cuIcon-search"></text>
				<input type="text" placeholder="搜索动作" />
				<text class="btn">搜索</text>
			</view>
		</view>

		<scroll-view scroll-x class="bg-white nav" scroll-with-animation :scroll-left="scrollLeft">
			<view class="cu-item" :class="index == selectIdx ? 'text-blue cur' : ''" v-for="(item, index) in 10" :key="index" @click="tabSelect" :data-id="index">
				Tab{{ index }}
			</view>
		</scroll-view>

		<view class="joints bg-white flex align-center">
			<view v-for="(item, index) in joints" :key="index" class="item">{{ item }}</view>
		</view>

		<scroll-view :style="{ height: scrollViewHeight }" id="scrollView" scroll-y="true">
			<view class="cu-list menu-avatar">
				<view
					class="cu-item "
					:class="modalName == 'move-box-' + index ? 'move-cur' : ''"
					v-for="(item, index) in list"
					:key="index"
					@touchstart="ListTouchStart"
					@touchmove="ListTouchMove"
					@touchend="ListTouchEnd"
					:data-target="'move-box-' + index"
				>
					<image class="cu-avatar lg" :src="item.logo" mode="aspectFill"></image>

					<view class="content">
						<view class="text-black">{{ item.name }}</view>

						<view class="text-gray text-sm">
							<text v-for="(itm, idx) in item.bodyParts" :key="idx" class=" text-gray  margin-right-xs">{{ itm }}</text>
							<text class="text-white">.</text>
						</view>
					</view>
					<view @click.stop="del(index)" class="move">
						<!-- <view class="bg-grey">置顶</view> -->
						<view class="bg-red">删除</view>
					</view>
				</view>
			</view>
		</scroll-view>
	</view>
</template>

<script>
export default {
	data() {
		return {
			selectIdx: 1,
			scrollLeft: 0,
			bodyParts: ['全部', '胸部', '背部', '肩部', '手臂', '颈部', '腹部', '腰部', '臀部', '腿部', '全身', '躯干'],
			joints: ['热身', '正式', '放松'],
			scrollViewHeight: 'calc(100vh - 180px)', // scrollview 高度

			modalName: null,
			listTouchStart: 0,
			listTouchDirection: null,
			isClick: false,

			list: [
				{ name: '哑铃推举', logo: '', bodyParts: ['肩部', '正式'] },
				{ name: '牛角包翻转', logo: '', bodyParts: [] },
				{ name: '牛角包', logo: '', bodyParts: ['躯干'] },
				{ name: '哑铃推举', logo: '', bodyParts: ['肩部', '正式'] },
				{ name: '牛角包翻转', logo: '', bodyParts: [] },
				{ name: '牛角包', logo: '', bodyParts: ['躯干'] },
				{ name: '哑铃推举', logo: '', bodyParts: ['肩部', '正式'] },
				{ name: '牛角包翻转', logo: '', bodyParts: [] },
				{ name: '牛角包', logo: '', bodyParts: ['躯干'] },
				{ name: '哑铃推举', logo: '', bodyParts: ['肩部', '正式'] },
				{ name: '牛角包翻转', logo: '', bodyParts: [] },
				{ name: '牛角包', logo: '', bodyParts: ['躯干'] },
				{ name: '哑铃推举', logo: '', bodyParts: ['肩部', '正式'] },
				{ name: '牛角包翻转', logo: '', bodyParts: [] },
				{ name: '牛角包', logo: '', bodyParts: ['躯干'] }
			]
		};
	},
	onReady() {
		const query = uni.createSelectorQuery().in(this);
		query
			.select('#scrollView')
			.boundingClientRect(data => {
				this.scrollViewHeight = `calc(100vh - ${data.top.toFixed(2)}px)`;
			})
			.exec();
	},
	methods: {
		gotoAddAction() {
			uni.navigateTo({
				url:"/pages/actionLibrary/addAction"
			})
		},
		del(idx) {
			uni.showModal({
				title: '删除动作',
				content: `确定删除${this.list[idx].name}吗?`
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
			if (this.listTouchDirection == 'left') {
				this.modalName = e.currentTarget.dataset.target;
			} else {
				this.modalName = null;
			}
			this.listTouchDirection = null;
		},
		tabSelect(e) {
			this.selectIdx = e.currentTarget.dataset.id;
			this.scrollLeft = (e.currentTarget.dataset.id - 1) * 60;
		}
	}
};
</script>

<style lang="scss">
.actionLibraryView {
	.searchBox {
		padding: 30rpx;
		.iptBox {
			height: 66rpx;
			background-color: #f6f6f6;
			border-radius: 16rpx;
			padding: 0 20rpx;
			input {
				font-size: 24rpx;
				flex: 1;
				margin-left: 14rpx;
			}
			.btn {
				font-size: 26rpx;
			}
		}
	}

	.joints {
		padding: 30rpx;
		margin-top: 10px;
		border-bottom: 1rpx solid #ededed;
		.item {
			height: 54rpx;
			line-height: 54rpx;
			padding: 0 24rpx;
			background-color: #f6f6f6;
			margin-right: 24rpx;
			font-size: 24rpx;
			border-radius: 10rpx;
		}
	}

	#scrollView {
		.cu-list > .cu-item.move-cur {
			transform: translateX(-70px);
		}
		.cu-list {
			.cu-item {
				border-bottom: none;
				.cu-avatar {
					width: 160rpx;
					border-radius: 6rpx;
					background-color: #f6f6f6;
				}
				.content {
					left: 230rpx;
				}
				.move {
					width: 70px;
				}
			}
		}
	}
}
</style>
