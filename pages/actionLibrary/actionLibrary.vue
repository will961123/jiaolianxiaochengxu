<template>
	<!-- 动作库 -->
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
					@click="gotoActionDetail"
					:data-target="'move-box-' + index"
				>
					<image class="cu-avatar lg" :src="item.logo" mode="aspectFill"></image>

					<view class="content flex justify-between">
						<view class="flex flex-direction">
							<view class="text-black">{{ item.name }}</view>
							<view class="text-gray text-sm">
								<text v-for="(itm, idx) in item.bodyParts" :key="idx" class=" text-gray  margin-right-xs">{{ itm }}</text>
								<text class="text-white">.</text>
							</view>
						</view>
						<view v-show="type === 'select'" class="numBox flex align-center">
							<view @click.stop="item.num--" v-show="item.num > 0" class="less roundBox"><text class="cuIcon cuIcon-move text-bold"></text></view>
							<view v-show="item.num > 0" class="num">{{ item.num }}</view>
							<view @click.stop="item.num++" class="add roundBox bg-blue"><text class="cuIcon cuIcon-add text-bold"></text></view>
						</view>
					</view>
					<view @click.stop="del(index)" class="move">
						<!-- <view class="bg-grey">置顶</view> -->
						<view class="bg-red">删除</view>
					</view>
				</view>
			</view>
		</scroll-view>

		<view v-show="type === 'select'" class="selectNumBox bg-white flex align-center justify-between">
			<view class="numBox flex1">
				动作数量：
				<text class="num text-blue">{{ selectNum }}</text>
			</view>
		<!-- 	<view class="rowBox flex1">
				内容
				<text class="cuIcon margin-left-sm cuIcon-unfold"></text>
			</view> -->
			<view @click="saveSelect" class="btn bg-blue flex1">确认</view>
		</view>
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
				{ name: '哑铃推举', logo: '', bodyParts: ['肩部', '正式'], num: 1 },
				{ name: '牛角包翻转', logo: '', bodyParts: [], num: 0 },
				{ name: '牛角包', logo: '', bodyParts: ['躯干'], num: 0 },
				{ name: '哑铃推举', logo: '', bodyParts: ['肩部', '正式'], num: 0 },
				{ name: '牛角包翻转', logo: '', bodyParts: [], num: 0 },
				{ name: '牛角包', logo: '', bodyParts: ['躯干'], num: 0 },
				{ name: '哑铃推举', logo: '', bodyParts: ['肩部', '正式'], num: 0 },
				{ name: '牛角包翻转', logo: '', bodyParts: [], num: 0 },
				{ name: '牛角包', logo: '', bodyParts: ['躯干'], num: 0 },
				{ name: '哑铃推举', logo: '', bodyParts: ['肩部', '正式'], num: 0 },
				{ name: '牛角包翻转', logo: '', bodyParts: [], num: 0 },
				{ name: '牛角包', logo: '', bodyParts: ['躯干'], num: 0 },
				{ name: '哑铃推举', logo: '', bodyParts: ['肩部', '正式'], num: 0 },
				{ name: '牛角包翻转', logo: '', bodyParts: [], num: 0 },
				{ name: '牛角包', logo: '', bodyParts: ['躯干'], num: 0 }
			],

			type: '',
			 
		};
	},
	computed: { 
		selectNum() {
			return this.list.reduce((t, v) => {
				return t + v.num;
			}, 0);
		}
	},
	onReady() {
		const query = uni.createSelectorQuery().in(this);
		query
			.select('#scrollView')
			.boundingClientRect(data => {
				this.scrollViewHeight = `calc(100vh - ${data.top.toFixed(2)}px)`;
				console.log(this.type);
				if (this.type === 'select') {
					this.scrollViewHeight = `calc(100vh - ${data.top.toFixed(2)}px - 50px)`;
				}
			})
			.exec();
	},
	onLoad(options) {
		if (options.type) {
			this.type = options.type;
		}
	},
	methods: {
		saveSelect(){
			uni.navigateBack({
				delta:1
			})
		},
		gotoActionDetail() {
			uni.navigateTo({
				url: '/pages/actionLibrary/actionDetail'
			});
		},
		gotoAddAction() {
			uni.navigateTo({
				url: '/pages/actionLibrary/addAction'
			});
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
					flex: 1;
					.numBox {
						.less {
							border: 1rpx solid #999999;
							color: #999;
						}
						.roundBox {
							width: 46rpx;
							height: 46rpx;
							line-height: 46rpx;
							border-radius: 50%;
							text-align: center;
						}
						.num {
							margin: 0 30rpx;
						}
					}
				}
				.move {
					width: 70px;
				}
			}
		}
	}

	.selectNumBox {
		width: 100%;
		height: 50px;
		position: fixed;
		left: 0;
		bottom: 0;
		border-top: 1rpx solid #999;
		padding-left: 35rpx;
		.numBox {
			color: #000;
			font-weight: 34rpx;
			.num {
				font-size: 38rpx;
			}
		}
		.rowBox {
			color: #999;
		}
		.btn {
			width: 34%;
			height: 50px;
			line-height: 50px;
			text-align: center;
			font-size: 28rpx;
		}
	}
}
</style>
