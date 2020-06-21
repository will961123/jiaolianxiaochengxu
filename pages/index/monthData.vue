<template>
	<!-- 月上课量 -->
	<view class="monthDataView">
		<cu-custom bgColor="bg-myblack" :isBack="true"><block slot="content">会员数据</block></cu-custom>

		<!-- 切换月份 -->
		<view class="dateBox">
			<picker mode="date" fields="month" :start="startDate" :end="endDate" @change="bindDateChange">
				<view class="date">
					{{ selectDate.year }}年{{ selectDate.month }}月
					<text class="margin-left-sm cuIcon cuIcon-unfold"></text>
				</view>
			</picker>
		</view>

		<!-- swiper & canvas -->
		<swiper class="swiper bg-white" :indicator-dots="true">
			<swiper-item>
				<view class="canvasDataBox flex justify-between ">
					<view class="left  flex flex-direction justify-between">
						<view class="itemTitle">完成课时</view>
						<view class="numBox">
							<text class="num">0</text>
							<text>节</text>
						</view>
						<view class="proportion">
							<text class="text-orange">
								同比上个月
								<text class="cuIcon cuIcon-refresharrow"></text>
								100%
							</text>
							<text v-if="false" class="text-green">同比上个月--</text>
						</view>
					</view>
					<view class="right">
						<view class="itemTitle">上课会员</view>
						<view class="flex justify-between align-center">
							<canvas canvas-id="canvasRing1" id="canvasRing1" class="charts"></canvas>
							<view class="legendBox flex flex-direction justify-between">
								<view v-for="(item, index) in canvasData.page1.data" :key="index" :class="[legendClass[index], 'item yellow flex justify-between']">
									<text>{{ item.name }}</text>
									<text>{{ item.data }}人</text>
								</view>
							</view>
						</view>
					</view>
				</view>
			</swiper-item>
			<swiper-item>
				<view class="canvasDataBox flex justify-between ">
					<view class="left  flex flex-direction justify-between">
						<view class="itemTitle">当前剩余/总课时</view>
						<view class="numBox">
							<text class="num">352</text>
							<text>429节</text>
						</view>
					</view>
					<view class="right">
						<view class="itemTitle">有课会员</view>
						<view class="flex justify-between align-center">
							<canvas canvas-id="canvasRing2" id="canvasRing2" class="charts"></canvas>
							<view class="legendBox flex flex-direction justify-between">
								<view class="legendBox flex flex-direction justify-between">
									<view v-for="(item, index) in canvasData.page1.data" :key="index" :class="[legendClass[index], 'item yellow flex justify-between']">
										<text>{{ item.name }}</text>
										<text>{{ item.data }}人</text>
									</view>
								</view>
							</view>
						</view>
					</view>
				</view>
			</swiper-item>
		</swiper>

		<view class="typeBox flex justify-between align-center">
			<view class="title">总会员 14 人</view>
			<view>
				<view @click="selectType = 0" :class="selectType === 0 ? 'item select' : 'item'">
					当前
					<br />
					总课时
				</view>
				<view @click="selectType = 1" :class="selectType === 1 ? 'item select' : 'item'">
					当前
					<br />
					剩余课时
				</view>
				<view @click="selectType = 2" :class="selectType === 2 ? 'item select' : 'item'">
					6月
					<br />
					消耗课时
				</view>
			</view>
		</view>
		
		<view class="bg-white scrollView">
			<view class="item flex align-center justify-between">
				<view class="flex align-center">
					<view class="idx">1</view>
					<view class="userBox flex align-center">
						<view class="head">德才</view>
						<view>
							<view class="name">杨德才</view>
							<view class="phone">139*****9197</view>
						</view>
					</view>
				</view>
			
				<view class="numBox flex align-center">
					<view class="item">81</view>
					<view class="item">81</view>
					<view class="item">81</view>
				</view>
			</view>
		</view>
	
	</view>
</template>

<script>
import uCharts from '@/components/u-charts/u-charts.js';
var _self;
var canvaRing = null;
export default {
	data() {
		return {
			cWidth: '',
			cHeight: '',
			pixelRatio: 1,
			canvasData: { page1: { data: [] }, page2: { data: [] } }, // 存放canvas模拟数据
			legendClass: ['skyblue', 'blue', 'yellow'],

			selectDate: this.getDate('month'),
			selectType: 0
		};
	},
	computed: {
		startDate() {
			return '2000-01-01';
		},
		endDate() {
			return this.getDate('end');
		}
	},
	onLoad() {
		this.cWidth = uni.upx2px(305);
		this.cHeight = uni.upx2px(250);
		// this.cWidth = 300;
		// this.cHeight = 200;
		this.getServerData();
	},
	methods: {
		// 发起请求
		getServerData() {
			/**
			 * 请求数据
			 */
			let data = {
				page1: {
					totNum: 15,
					data: [
						{
							name: '消耗>12节',
							data: 1
						},
						{
							name: '消耗5-12节',
							data: 2
						},
						{
							name: '消耗1-4节',
							data: 3
						}
					]
				},
				page2: {
					totNum: 11,
					data: [
						{
							name: '剩余>30节',
							data: 1
						},
						{
							name: '剩余10-30节',
							data: 2
						},
						{
							name: '剩余<10节',
							data: 3
						}
					]
				}
			};
			this.canvasData = data;
			/**
			 * canvas:canvas id
			 * series:要渲染的数据
			 */
			let page1Config = {
				canvas: 'canvasRing1',
				totNum: this.canvasData.page1.totNum,
				series: this.canvasData.page1.data
			};
			let page2Config = {
				canvas: 'canvasRing2',
				totNum: this.canvasData.page2.totNum,
				series: this.canvasData.page2.data
			};
			this.showRing(page1Config);
			this.showRing(page2Config);
		},
		// 初始化canvas
		showRing(config) {
			let that = this;
			canvaRing = new uCharts({
				$this: that,
				canvasId: config.canvas,
				type: 'ring',
				fontSize: 10,
				// legend: true,
				// 右侧配置
				// 配置为不显示右侧图例 用dom生成 不然canvas太宽了
				legend: {
					show: false,
					position: 'right',
					float: 'center',
					itemGap: 10,
					padding: 5,
					lineHeight: 26,
					margin: 5,
					borderWidth: 1
				},
				title: {
					name: config.totNum + '人',
					color: '#000000',
					fontSize: 12 * that.pixelRatio,
					offsetY: -4 * that.pixelRatio
				},
				// subtitle: {
				// 	name: '收益率',
				// 	color: '#666666',
				// 	fontSize: 15 * that.pixelRatio,
				// 	offsetY: 30 * that.pixelRatio
				// },
				extra: {
					pie: {
						offsetAngle: -45,
						// 圆环宽度
						ringWidth: 18 * that.pixelRatio,
						// 标线
						labelWidth: 10,
						border: true
					}
				},
				background: '#FFFFFF',
				pixelRatio: that.pixelRatio,
				series: config.series,
				animation: true,
				width: that.cWidth * that.pixelRatio,
				height: that.cHeight * that.pixelRatio,
				disablePieStroke: true,
				// 圆环外的标线
				dataLabel: false
			});
		},
		// 没用
		touchRing(e) {
			// canvas 太小了
			return;
			canvaRing.showToolTip(e, {
				format: function(item) {
					return item.name + ':' + item.data;
				}
			});
		},

		getDate(type) {
			const date = new Date();
			let year = date.getFullYear();
			let month = date.getMonth() + 1;
			let day = date.getDate();

			if (type === 'start') {
				year = year - 60;
			} else if (type === 'end') {
				year = year;
			}
			month = month > 9 ? month : '0' + month;
			day = day > 9 ? day : '0' + day;

			if (type === 'month') {
				return { year, month };
			} else {
				return `${year}-${month}-${day}`;
			}
		},
		bindDateChange(e) {
			let date = e.detail.value.split('-');
			this.selectDate = { year: date[0], month: date[1] };
		}
	}
};
</script>

<style lang="scss">
.monthDataView {
	.dateBox {
		.date {
			font: 34rpx;
			color: #fff;
			background-color: #4c515f;
			line-height: 40px;
			padding: 0 30rpx;
			letter-spacing: 5rpx;
		}
	}

	.swiper {
		height: 400rpx;

		.canvasDataBox {
			padding: 40rpx 15rpx 0 35rpx;
			.itemTitle {
				font-size: 28rpx;
				color: #000;
				font-weight: 700;
				padding-left: 20rpx;
				position: relative;
				line-height: 40rpx;
				&::after {
					position: absolute;
					left: 0;
					top: 0;
					width: 10rpx;
					height: 40rpx;
					background-color: #1a8ee9;
					content: '';
				}
			}
			.left {
				width: 300rpx;
				.numBox {
					font-size: 24rpx;
					color: #999;
					.num {
						font-size: 36rpx;
						color: #000;
						font-weight: 700;
						margin-right: 6rpx;
					}
				}
				.proportion {
					margin-bottom: 40rpx;
				}
			}
			.right {
				flex: 1;
				.charts {
					width: 255rpx;
					height: 250rpx;
					background-color: #ffffff;
					margin-left: -20px;
				}
				.legendBox {
					font-size: 22rpx;
					height: 160rpx;
					flex: 1;
					.item {
						padding-left: 28rpx;
						position: relative;
						&::after {
							width: 16rpx;
							height: 16rpx;
							content: '';
							position: absolute;
							left: 0;
							top: 4rpx;
						}
						&.yellow {
							&::after {
								background-color: #facc14;
							}
						}
						&.blue {
							&::after {
								background-color: #2fc25b;
							}
						}
						&.skyblue {
							&::after {
								background-color: #1890ff;
							}
						}
					}
				}
			}
		}
	}

	.typeBox {
		background: #f6f6f6;
		padding: 16rpx 15rpx 16rpx 25rpx;
		font-size: 28rpx;
		color: #a4a4a4;

		.item {
			font-size: 22rpx;
			border-radius: 40rpx;
			display: inline-block;
			padding: 4px 20rpx;
			text-align: center;
			margin-left: 4px;
			line-height: 1.2em;
			// letter-spacing: 4rpx;
			&.select {
				background-color: #1a8ee9;
				color: #fff;
			}
		}
	}
	
	.scrollView{
		&>.item{
			border-bottom: 1rpx solid #ededed;
			padding: 30rpx 0;
			font-size: 24rpx;
			.idx{
					margin: 0 15rpx;
					width: 36rpx;
					height: 36rpx;
					line-height: 36rpx;
					background-color: red;
					text-align: center; 
					border-radius: 8rpx; 
			}
			.userBox{
				.head{
					background-color: red;
					width: 64rpx;
					height: 64rpx;
					line-height: 64rpx;
					border-radius: 50%;
					text-align: center;
					margin-right: 8rpx;
					color: #fff;
				}
				.name{
					font-size: 28rpx;
					margin-bottom: 6rpx;
				}
				.phone{
					margin-left: 6rpx;
				}
			}
			.numBox{
				flex: 1;
				margin-left: 40rpx;
				.item{
					width: 32%;
					text-align: center;
				}
			}
		}
	}
}
</style>
