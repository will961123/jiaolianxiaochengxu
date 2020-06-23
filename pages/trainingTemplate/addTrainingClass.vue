<template>
	<!-- 添加训练课 -->
	<view class="addTrainingClassView">
		<view class="title bg-white flex align-center"><input type="text" value="" placeholder="请输入训练主题" /></view>
		<view class="section bg-white">
			<view class="titBox flex justify-between align-center">
				<view class="flex align-center">
					<view style="background-color: #fdb146;" class="label "></view>
					<view class="tit">热身训练</view>
				</view>
				<view class="addCycle">创建循环</view>
			</view>

			<view v-for="(item, index) in detailInfo.warmUp" :key="index" class="listBox">
				<view class="list flex justify-between align-center">
					<view class="flex align-center">
						<view class="name">{{ item.name }}</view>
						<view class="num flex">
							<view class="left">{{ item.list.length }}组x3次</view>
							<view class="desc">
								<text>{{ item.list[0].equipment }}</text>
								<text>{{ item.list[0].kg }}</text>
								<text>{{ item.list[0].kgType }}</text>
								<text>间休{{ item.list[0].rest }}s</text>
							</view>
						</view>
					</view>
					<view @click="item.select = !item.select" :class="item.select ? 'cuIcon-fold' : 'cuIcon-unfold'" class="cuIcon "></view>
				</view>
				<view v-show="item.select" class="parameterBox">
					<view v-for="(itm, idx) in item.list" :key="idx" class="item">
						<text>{{ itm.num }}次</text>
						<text>{{ itm.equipment }}</text>
						<text>{{ itm.kg }}{{ itm.kgType }}</text>
						<text class="no-border">休息{{ itm.rest }}</text>
						<view @click="delOneParamete(index, idx, 'warmUp')" class=" close cuIcon cuIcon-roundclose"></view>
					</view>
				</view>
			</view>

			<view @click="gotoActionLibrary" class="addBox text-blue">
				<text class="cuIcon cuIcon-add"></text>
				添加动作
			</view>
		</view>

		<view class="section bg-white">
			<view class="titBox flex justify-between align-center">
				<view class="flex align-center">
					<view style="background-color: #4bd3c4;" class="label  "></view>
					<view class="tit">正式训练</view>
				</view>
				<view class="addCycle">创建循环</view>
			</view>
			<view @click="gotoActionLibrary" class="addBox text-blue">
				<text class="cuIcon cuIcon-add"></text>
				添加动作
			</view>
		</view>

		<view class="section bg-white">
			<view class="titBox flex justify-between align-center">
				<view class="flex align-center">
					<view style="background-color: #799bf1;" class="label  "></view>
					<view class="tit">放松整理</view>
				</view>
				<view class="addCycle">创建循环</view>
			</view>
			<view @click="gotoActionLibrary" class="addBox text-blue">
				<text class="cuIcon cuIcon-add"></text>
				添加动作
			</view>
		</view>
	</view>
</template>

<script>
export default {
	data() {
		return {
			detailInfo: {
				warmUp: [
					{
						name: '哑铃推举',
						select: false,
						list: [
							{ num: 3, equipment: -1, kg: -1, kgType: 0, rest: -1 },
							{ num: 4, equipment: -1, kg: -1, kgType: 0, rest: -1 },
							{ num: 5, equipment: -1, kgType: 0, rest: -1 }
						]
					},
					{
						name: '牛角包翻转',
						select: false,
						list: [
							{ num: 3, equipment: -1, kg: -1, kgType: 0, rest: -1 },
							{ num: 4, equipment: -1, kg: -1, kgType: 0, rest: -1 },
							{ num: 5, equipment: -1, kgType: 0, rest: -1 }
						]
					},
					{
						name: '牛角包',
						select: false,
						list: [
							{ num: 3, equipment: -1, kg: -1, kgType: 0, rest: -1 },
							{ num: 4, equipment: -1, kg: -1, kgType: 0, rest: -1 },
							{ num: 5, equipment: -1, kgType: 0, rest: -1 }
						]
					}
				]
			}
		};
	},
	methods: {
		delOneParamete(index, idx, key) {
			if (this.detailInfo[key][index].list.length <= 1) {
				return;
			}
			this.detailInfo[key][index].list.splice(idx, 1);
		},
		gotoActionLibrary() {
			uni.navigateTo({
				url: '/pages/actionLibrary/actionLibrary?type=select'
			});
		}
	}
};
</script>

<style lang="scss">
.addTrainingClassView {
	& > .title {
		height: 50px;
		padding: 0 30rpx;
		margin-bottom: 24rpx;
		input {
			font-size: 28rpx;
		}
	}
	.section {
		margin-bottom: 18px;
		.titBox {
			padding: 30rpx;
			height: 50px;
			border-bottom: 1rpx solid #ededed;
			.label {
				width: 24rpx;
				height: 24rpx;
				margin-right: 25rpx;
			}
			.tit {
				font-size: 30rpx;
				color: #000;
			}
			.addCycle {
				color: #999;
				border-radius: 8rpx;
				padding: 12rpx 18rpx;
				font-size: 26rpx;
				border: 1rpx solid #cfcfcf;
			}
		}
		.listBox {
			padding: 0 30rpx;
			margin-bottom: 10px;
			.list {
				height: 55px;
				background-color: #f6f6f6;
				border-radius: 8rpx;

				padding: 0 20rpx 0 50rpx;
				.name {
					margin-right: 14rpx;
					font-size: 28rpx;
				}
				.num {
					font-size: 24rpx;
					line-height: 30px;
					padding-right: 15rpx;
					border: 1px solid #8695a4;
					color: #8695a4;
					.left {
						background-color: #8695a4;
						color: #fff;
						padding-left: 15rpx;
						padding-right: 10rpx;
					}
					.desc {
						padding-left: 8rpx;
						& > text {
							margin-right: 12rpx;
						}
					}
				}
				.cuIcon {
					font-size: 32rpx;
				}
			}

			.parameterBox {
				background-color: #f6f6f6;
				padding-left: 50rpx;
				border-radius: 8rpx;
				margin-top: 14px;
				padding-top: 20px;
				.item {
					padding-bottom: 35px;
					position: relative;

					&::after {
						width: 10rpx;
						height: 10rpx;
						border-radius: 50%;
						position: absolute;
						top: 0;
						left: 0;
						margin-top: 14rpx;
						content: '';
						background-color: #8695a4;
					}
					& > text {
						color: #333333;
						font-size-adjust: 22rpx;
						display: inline-block;
						width: 20%;
						text-align: center;
						border-right: 1rpx solid #666;
						&:last-child {
							border: none;
						}
					}
					.no-border {
						border: none;
					}
					.close {
						display: inline-block;
						font-size: 32rpx;
						margin-left: 10rpx;
					}
				}
			}
		}
		.addBox {
			height: 70px;
			line-height: 70px;
			text-align: center;
		}
	}
}
</style>
