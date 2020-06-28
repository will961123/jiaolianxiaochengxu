<template>
	<!-- 课程详情 -->
	<view class="classDetailView">
		<view class="title bg-white flex align-center justify-between">
			<input type="text" class="text-black text-black" :disabled="true" value="肩背力量训练" />
			<view @click="gotoBeforeClass" class="btnBox bg-red flex align-center justify-center"><view class="btn bg-blue">去上课</view></view>
		</view>
		<view class="section bg-white">
			<view class="titBox flex justify-between align-center">
				<view class="flex align-center">
					<view style="background-color: #fdb146;" class="label "></view>
					<view class="tit">热身训练</view>
				</view>
			</view>

			<view @click="showModal" data-target="Detail" v-for="(item, index) in detailInfo.warmUp" :key="index" class="listBox">
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
				</view>
			</view>
		</view>

		<view class="section bg-white">
			<view class="titBox flex justify-between align-center">
				<view class="flex align-center">
					<view style="background-color: #4bd3c4;" class="label  "></view>
					<view class="tit">正式训练</view>
				</view>
			</view>
			<view class="nodata">暂无动作</view>
		</view>

		<view class="section bg-white">
			<view class="titBox flex justify-between align-center">
				<view class="flex align-center">
					<view style="background-color: #799bf1;" class="label  "></view>
					<view class="tit">放松整理</view>
				</view>
			</view>
			<view class="nodata">暂无动作</view>
		</view>

		<!-- 弹窗-->
		<view class="cu-modal bottom-modal " :class="modalName == 'Detail' ? 'show' : ''" @tap="hideModal">
			<view style="padding: 0;" class="cu-dialog bg-white" @tap.stop="">
				<view class="banner">
					<view @tap="hideModal" class="hideBox">
						<text class="cuIcon cuIcon-unfold text-white"> </text>
					</view>
					<image src="" mode="aspectFill"></image>
				</view>
				<view style="padding: 0 30rpx;">
					<view class="calssName">T字下拉</view>
					<view class="tit">动作介绍</view>
					<view class="desc">无</view>
					<view class="tit">动作步骤</view>
					<view class="desc">无</view>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
export default {
	data() {
		return {
			focus: false,
			focus2: false,
			detailInfo: {
				warmUp: [
					{
						name: '哑铃推举',
						select: true,
						list: [
							{ num: 3, numType: 0, equipment: '哑铃', kg: 1, kgType: 'KG', rest: -1 },
							{ num: 4, numType: 0, equipment: '哑铃', kg: 1, kgType: 'KG', rest: -1 },
							{ num: 5, numType: 0, equipment: '哑铃', kg: 1, kgType: 'KG', rest: -1 }
						]
					},
					{
						name: '牛角包翻转',
						select: false,
						list: [
							{ num: 3, numType: 0, equipment: '哑铃', kg: 1, kgType: 'KG', rest: -1 },
							{ num: 4, numType: 0, equipment: '哑铃', kg: 1, kgType: 'KG', rest: -1 },
							{ num: 5, numType: 0, equipment: '哑铃', kg: 1, kgType: 'KG', rest: -1 }
						]
					},
					{
						name: '牛角包',
						select: false,
						list: [
							{ num: 3, numType: 0, equipment: '哑铃', kg: 1, kgType: 'KG', rest: -1 },
							{ num: 4, numType: 0, equipment: '哑铃', kg: 1, kgType: 'KG', rest: -1 },
							{ num: 5, numType: 0, equipment: '哑铃', kg: 1, kgType: 'KG', rest: -1 }
						]
					}
				]
			},
			modalName: ''
		};
	},
	onLoad() {},
	methods: {
		gotoBeforeClass(){
			uni.navigateTo({
				url:"/pages/trainingPlan/beforeClass"
			})
		},
		showModal(e) {
			this.modalName = e.currentTarget.dataset.target;
			if (this.modalName === 'Resistance') {
				this.focus = true;
			} else if (this.modalName === 'Rest') {
				this.focus2 = true;
			}
		},
		hideModal(e) { 
			this.modalName = null;
			this.focus = false;
			this.focus2 = false;

		}
	}
};
</script>

<style lang="scss">
.classDetailView {
	& > .title {
		height: 58px;
		padding: 0 30rpx;
		margin-bottom: 24rpx;
		input {
			font-size: 34rpx;
			color: #000;
			font-weight: 700;
		}
	}
	.btnBox {
		width: 170rpx;
		height: 44px;
		border-radius: 22px;
		background-color: rgba(0, 129, 255, 0);
		animation: togOpacity 2s infinite;
		.btn {
			width: 130rpx;
			text-align: center;
			height: 30px;
			line-height: 30px;
			border-radius: 15px;
		}
	}
	@keyframes togOpacity {
		from {
			// width: 130rpx;
			// height: 30px;
			// border-radius: 15px;
			background-color: rgba(0, 129, 255, 0.5);
		}
		to {
			// width: 170rpx;
			// height: 44px;
			// border-radius: 22px;
			background-color: rgba(0, 129, 255, 0);
		}
	}
	.section {
		margin-bottom: 18px;
		min-height: 80px;
		.nodata {
			line-height: 44px;
			text-align: center;
			color: #999;
			font-size: 24rpx;
			border-top: 1rpx solid #ededed;
		}
		.titBox {
			padding: 30rpx;
			height: 50px;

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
						// color: #333333;
						font-size-adjust: 22rpx;
						display: inline-block;
						width: 20%;
						text-align: center;
						border-right: 1rpx solid #dddddd;
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

	.cu-modal {
		z-index: 999 !important;
		.cu-dialog {
			height: 70vh;
			text-align: left;
			.banner {
				width: 100%;
				height: 400rpx;
				background-color: #1a8ee9;
				position: relative;
				& > image {
					width: 100%;
					height: 100%;
				}
				.hideBox{
					border-radius: 50%;
					width: 70rpx;
					height: 70rpx;
					background-color: rgba(0,0,0,0.2);
					font-size: 38rpx;
					line-height: 70rpx;
					text-align: center;
					position: absolute;
					left: 30rpx;
					top: 30rpx;
					z-index: 1234;
				}
			}
			.calssName {
				font-size: 38rpx;
				line-height: 55px;
				border-bottom: 1rpx solid #ededed;
				color: #000;
			}
			.tit {
				font-size: 28rpx;
				margin-top: 20px;
				color: #000;
			}
			.desc {
				color: #999;
				font-size: 24rpx;
				margin: 10px 0;
			}
		}
	}
}
</style>
