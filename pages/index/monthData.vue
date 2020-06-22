<template>
	<!-- 月上课量 -->
	<view class="monthDataView">
		<!-- <cu-custom bgColor="bg-myblack" :isBack="true"><block slot="content">会员数据</block></cu-custom> -->

		<!-- 切换月份 -->
		<view :style="{ top: 0 + 'px' }" class="dateBox">
			<picker mode="date" fields="month" :start="startDate" :end="endDate" @change="bindDateChange">
				<view class="date">
					{{ selectDate.year }}年{{ selectDate.month }}月
					<text class="margin-left-sm cuIcon cuIcon-unfold"></text>
				</view>
			</picker>
		</view>

		<!-- swiper & canvas -->
		<swiper indicator-active-color="#adadad" indicator-color="#eaeaea" :style="{ 'margin-top': 40 + 'px' }" class="swiper bg-white" :indicator-dots="true">
			<swiper-item>
				<view class="canvasDataBox flex justify-between ">
					<view class="left  flex flex-direction justify-between">
						<view class="itemTitle">完成课时</view>
						<view class="numBox">
							<text class="num">{{ canvasData.page1.useNum }}</text>
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
							<view class="canvasBox"><canvas v-show="canShowCanvas" canvas-id="canvasRing1" id="canvasRing1" class="charts"></canvas></view>

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
							<text class="num">{{ canvasData.page2.nowNum }}</text>
							<text>{{ '/' + canvasData.page2.totStudyNum }}节</text>
						</view>
					</view>
					<view class="right">
						<view class="itemTitle">有课会员</view>
						<view class="flex justify-between align-center">
							<view class="canvasBox"><canvas v-show="canShowCanvas" canvas-id="canvasRing2" id="canvasRing2" class="charts"></canvas></view>

							<view class="legendBox flex flex-direction justify-between">
								<view class="legendBox flex flex-direction justify-between">
									<view v-for="(item, index) in canvasData.page2.data" :key="index" :class="[legendClass[index], 'item yellow flex justify-between']">
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

		<view :style="{ top: 0 + 40 + 'px' }" class="typeBox sticky-box flex justify-between align-center">
			<view class="title">总会员 {{userList[selectType].length}} 人</view>
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
					{{ selectDate.month }}月
					<br />
					消耗课时
				</view>
			</view>
		</view>

		<!-- 需要使用scrollview 的话就把view 换成scrollview -->
		<view id="scrollView" class="bg-white scrollView">
			<!-- <scroll-view :style="{ height: scrollViewHeight }" id="scrollView" class="bg-white scrollView" scroll-y="true"> -->
			<view v-for="(item, index) in userList[selectType]" :key="index" class="item flex align-center justify-between">
				<view class="flex align-center">
					<view :style="{ 'background-color': collerList[index] || 'transparent', color: index > 2 ? '#666' : '#fff' }" class="idx">{{ index + 1 }}</view>
					<view class="userBox flex align-center">
						<view class="head  ">{{ item.name[item.name.length - 2] + item.name[item.name.length - 1] }}</view>
						<view>
							<view class="name ">{{ item.name }}</view>
							<view class="phone">{{ item.phone }}</view>
						</view>
					</view>
				</view>

				<view class="numBox flex align-center">
					<view class="num">{{ item.tot }}</view>
					<view class="num">{{ item.now }}</view>
					<view class="num">{{ item.use }}</view>
				</view>
			</view>
			<!-- </scroll-view> -->
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
			userList: [[], [], []], // 存放请求的数据
			canvasData: { page1: { data: [] }, page2: { data: [] } }, // 存放canvas模拟数据
			legendClass: ['skyblue', 'blue', 'yellow'],
			canvasTop: 100, // canvas 右侧图例距离顶部高度
			canShowCanvas: true, // canvas是原生组件 层级太高 无法覆盖 判断下滚动高度是否显示

			selectDate: this.getDate('month'),
			selectType: 0,
			collerList: ['#c9ab82', '#a9b0ba', '#86babe'],
			scrollViewHeight: '30vh', // scrollview 高度
			CustomBar: this.CustomBar
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
	onReady() {
		const query = uni.createSelectorQuery().in(this);
		query
			.select('#scrollView')
			.boundingClientRect(data => {
				this.scrollViewHeight = 'calc(100vh - ' + data.top.toFixed(2) + 'px)';
				console.log(this.scrollViewHeight);
			})
			.exec();
		query
			.select('.legendBox')
			.boundingClientRect(data => {
				this.canvasTop = data.top.toFixed(2);
			})
			.exec();
	},
	onPageScroll(res) {
		if (res.scrollTop + 0 + 40 > this.canvasTop) {
		// if (res.scrollTop + this.CustomBar + 40 > this.canvasTop) {
			console.log('隐藏canvas');
			this.canShowCanvas = false;
		} else {
			this.canShowCanvas = true;
		}
	},
	methods: {
		// 发起请求 初始化数据
		getServerData() {
			/**
			 * 请求数据
			 * 按照3种类型排序好的
			 */
			let resData = [
				[
					{ name: '杨德才', phone: '139****9197', tot: 81, now: 22, use: 3 },
					{ name: '杨德才', phone: '139****9197', tot: 81, now: 22, use: 3 },
					{ name: '杨德才', phone: '139****9197', tot: 81, now: 22, use: 3 },
					{ name: '张楠', phone: '139****9197', tot: 42, now: 71, use: 16 },
					{ name: '张楠', phone: '139****9197', tot: 42, now: 71, use: 16 },
					{ name: '张楠', phone: '139****9197', tot: 42, now: 71, use: 16 },
					{ name: '黄洋洋', phone: '139****9197', tot: 38, now: 16, use: 16 },
					{ name: '黄洋洋', phone: '139****9197', tot: 38, now: 16, use: 16 },
					{ name: '黄洋洋', phone: '139****9197', tot: 38, now: 16, use: 16 },
					{ name: '阮林平', phone: '139****9197', tot: 37, now: 8, use: 6 },
					{ name: '阮林平', phone: '139****9197', tot: 37, now: 8, use: 6 },
					{ name: '阮林平', phone: '139****9197', tot: 37, now: 8, use: 6 },
					{ name: '石金华', phone: '139****9197', tot: 32, now: 16, use: 12 },
					{ name: '石金华', phone: '139****9197', tot: 32, now: 16, use: 12 },
					{ name: '石金华', phone: '139****9197', tot: 32, now: 16, use: 12 }
				],
				[
					{ name: '张楠', phone: '139****9197', tot: 42, now: 71, use: 16 },
					{ name: '张楠', phone: '139****9197', tot: 42, now: 71, use: 16 },
					{ name: '张楠', phone: '139****9197', tot: 42, now: 71, use: 16 },
					{ name: '杨德才', phone: '--', tot: 81, now: 22, use: 3 },
					{ name: '杨德才', phone: '--', tot: 81, now: 22, use: 3 },
					{ name: '杨德才', phone: '--', tot: 81, now: 22, use: 3 },
					{ name: '黄洋洋', phone: '139****9197', tot: 38, now: 16, use: 16 },
					{ name: '黄洋洋', phone: '139****9197', tot: 38, now: 16, use: 16 },
					{ name: '黄洋洋', phone: '139****9197', tot: 38, now: 16, use: 16 },
					{ name: '石金华', phone: '139****9197', tot: 32, now: 16, use: 12 },
					{ name: '石金华', phone: '139****9197', tot: 32, now: 16, use: 12 },
					{ name: '石金华', phone: '139****9197', tot: 32, now: 16, use: 12 },
					{ name: '阮林平', phone: '139****9197', tot: 37, now: 8, use: 6 },
					{ name: '阮林平', phone: '139****9197', tot: 37, now: 8, use: 6 },
					{ name: '阮林平', phone: '139****9197', tot: 37, now: 8, use: 6 }
				],
				[
					{ name: '张楠', phone: '139****9197', tot: 42, now: 71, use: 16 },
					{ name: '张楠', phone: '139****9197', tot: 42, now: 71, use: 16 },
					{ name: '张楠', phone: '139****9197', tot: 42, now: 71, use: 16 },
					{ name: '黄洋洋', phone: '139****9197', tot: 38, now: 16, use: 16 },
					{ name: '黄洋洋', phone: '139****9197', tot: 38, now: 16, use: 16 },
					{ name: '黄洋洋', phone: '139****9197', tot: 38, now: 16, use: 16 },
					{ name: '石金华', phone: '139****9197', tot: 32, now: 16, use: 12 },
					{ name: '石金华', phone: '139****9197', tot: 32, now: 16, use: 12 },
					{ name: '石金华', phone: '139****9197', tot: 32, now: 16, use: 12 },
					{ name: '阮林平', phone: '139****9197', tot: 37, now: 8, use: 6 },
					{ name: '阮林平', phone: '139****9197', tot: 37, now: 8, use: 6 },
					{ name: '阮林平', phone: '139****9197', tot: 37, now: 8, use: 6 },
					{ name: '杨德才', phone: '--', tot: 81, now: 22, use: 3 },
					{ name: '杨德才', phone: '--', tot: 81, now: 22, use: 3 },
					{ name: '杨德才', phone: '--', tot: 81, now: 22, use: 3 }
				]
			];
			this.userList = resData;
			// 处理成canvas需要的格式  只需要一种排序好的数组
			let userList = resData[0];
			let page1 = {
				totNum: 0,
				useNum: 0, //完成课时
				data: [{ name: '消耗>12节', data: 0 }, { name: '消耗5-12节', data: 0 }, { name: '消耗1-4节', data: 0 }]
			};
			let page2 = {
				totNum: 0,
				nowNum: 0, //当前剩余/总课时
				totStudyNum: 0,
				data: [{ name: '剩余>30节', data: 0 }, { name: '剩余10-30节', data: 0 }, { name: '剩余<10节', data: 0 }]
			};
			for (let i in userList) {
				if (userList[i].use > 0 && userList[i].use <= 4) {
					page1.data[2].data += 1;
				} else if (userList[i].use > 4 && userList[i].use <= 12) {
					page1.data[1].data += 1;
				} else if (userList[i].use > 12) {
					page1.data[0].data += 1;
				}

				page1.useNum += userList[i].use;

				if (userList[i].now < 10) {
					page2.data[2].data += 1;
				} else if (userList[i].now >= 10 && userList[i].now <= 30) {
					page2.data[1].data += 1;
				} else if (userList[i].now > 30) {
					page2.data[0].data += 1;
				}
				page2.nowNum += userList[i].now;
				page2.totStudyNum += userList[i].tot;
			}
			page1.totNum = page1.data.reduce((t, v) => {
				return (t += v.data);
			}, 0);
			page2.totNum = page2.data.reduce((t, v) => {
				return (t += v.data);
			}, 0);

			let data = {
				page1,
				page2
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
.sticky-box {
	/* #ifndef APP-PLUS-NVUE */
	display: flex;
	position: -webkit-sticky;
	/* #endif */
	position: sticky;
	z-index: 99;
	flex-direction: row;
}

.monthDataView {
	.dateBox {
		position: fixed;
		width: 100%;
		left: 0;
		z-index: 9;
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
				.canvasBox {
					width: 255rpx;
					height: 250rpx;
					margin-left: -20px;
				}
				.charts {
					width: 255rpx;
					height: 250rpx;
					background-color: #ffffff;
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
								background-color: #26b9a0;
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

	.scrollView {
		.item {
			border-bottom: 1rpx solid #ededed;
			padding: 30rpx 0;
			font-size: 24rpx;
			.idx {
				margin: 0 15rpx;
				width: 36rpx;
				height: 36rpx;
				line-height: 36rpx;
				text-align: center;
				border-radius: 8rpx;
			}
			.userBox {
				width: 230rpx;
				.head {
					background-color: #26b9a0;
					width: 68rpx;
					height: 68rpx;
					line-height: 68rpx;
					border-radius: 50%;
					text-align: center;
					margin-right: 8rpx;
					color: #fff;
				}
				.name {
					font-size: 28rpx;
					margin-bottom: 6rpx;
				}
				.phone {
					margin-left: 6rpx;
				}
			}
			.numBox {
				flex: 1;
				margin-left: 40rpx;
				font-size: 28rpx;
				.num {
					width: 32%;
					text-align: center;
				}
			}
		}
	}
}
</style>
