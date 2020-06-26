<template>
	<!-- 课程总结 -->
	<view class="courseSummaryView">
		<view class="topBox flex align-center">
			<view class="headBox   ">韩晓</view>
			<view class="rightBox">
				<view class="rTop  flex align-center">
					<text>韩晓</text>
					<view class="Id">0139100325</view>
				</view>
				<view class="rBottom flex align-center">
					<view class="phone">186****2655</view>
					<view class="sex">女</view>
					<view class="age">29</view>
				</view>
			</view>
		</view>

		<view class="classInfo flex justify-between align-center bg-white">
			<view class="userType flex align-center">
				<text>客户心情</text>
				<image src="/static/pic3.png" mode="aspectFill"></image>
			</view>
			<view class="time">
				<text>课程时长:</text>
				<text class="text-bold">04:58</text>
			</view>
			<view class="date">
				<text>完成日期:</text>
				<text class="text-bold">06:17</text>
			</view>
		</view>

		<!-- 课程内容 -->
		<view class="classDetail bg-white">
			<view class="descView bg-blue">请教练确认</view>
			<view style="padding: 0 25rpx;">
				<view class="titBox flex justify-between align-center">
					<view class="tit">
						课程内容
						<text class="desc">请为动作选择完成情况，未标记的视为未执行</text>
					</view>
					<text class="text-black">查看计划</text>
				</view>

				<view
					v-for="(item, index) in list"
					:key="index"
					:class="index <= 1 ? 'itemyellow' : index <= 3 ? 'itemblue' : index <= 5 ? 'itempurple' : 'itemyellow'"
					class="item"
				>
					<view class="itemcontent bg-grey flex justify-between align-center text-black">
						<view class="flex align-center">
							<text class="margin-left-sm">{{ item.name }}</text>
							<text class="margin-left-sm">{{ item.num }}</text>
							<view v-if="item.dot" class="dot margin-left-sm"></view>
						</view>
						<view class="picBox">
							<view class="flex align-center">
								<image
									@click="changeUserType(index, idx)"
									v-show="item.userType == -1"
									v-for="(pic, idx) in picList"
									:key="idx"
									:src="'/static/pic' + (idx + 1) + '.png'"
									mode="aspectFill"
								></image>
								<image v-show="item.userType != -1" :src="'/static/pic' + item.userType + '.png'" mode="aspectFill"></image>
							</view>
						</view>
					</view>
				</view>
			</view>

			<!-- 客户确认 -->
			<view class="userSave">
				<view class="descView bg-blue">请客户确认</view>
				<view class="box">
					<view class="flex justify-between">
						<view class="item flex justify-between align-center">
							<view class="name">客户体重</view>
							<view style="flex: 1;margin-left: 8rpx;" class="flex align-center justify-between">
								<input type="text" value="" />
								<text class="text-grey">kg</text>
							</view>
						</view>
						<view class="item flex justify-between align-center">
							<view class="name">睡眠时长</view>
							<view style="flex: 1;margin-left: 8rpx;" class="flex align-center justify-between">
								<input type="text" value="" />
								<text class="text-grey">h</text>
							</view>
						</view>
					</view>
					<view style="margin-top: 8px;" class="flex justify-between">
						<view class="item flex justify-between align-center">
							<view class="name">胃口指数</view>
							<view style="flex: 1;margin-left: 8rpx;position: relative;" class="flex align-center justify-between">
								<input type="text" disabled="true" :value="appetiteLv > -1 ? LvList[appetiteLv] : '请选择'" />
								<text class="text-grey cuIcon cuIcon-right "></text>
								<picker mode="selector" :range="LvList" @change="bindPickerChange($event, 1)">请选择</picker>
							</view>
						</view>
						<view class="item flex justify-between align-center">
							<view class="name">活力指数</view>
							<view style="flex: 1;margin-left: 8rpx;position: relative;" class="flex align-center justify-between">
								<input type="text" disabled="true" :value="vitalityLv > -1 ? LvList[vitalityLv] : '请选择'" />
								<text class="text-grey cuIcon cuIcon-right "></text>
								<picker mode="selector" :range="LvList" @change="bindPickerChange($event, 2)">请选择</picker>
							</view>
						</view>
					</view>
				</view>
				
				<view class="userDesc">
					<textarea value="" placeholder="说说你的感受" />
				</view>
			</view>
		</view>
		
		<view class="btn bg-blue text-center">
			确定
		</view>
	</view>
</template>

<script>
export default {
	data() {
		return {
			list: [
				{ name: '伸展', num: '4/4组', dot: false, userType: 1 },
				{ name: '伸展', num: '4/4组', dot: false, userType: 2 },
				{ name: '伸展', num: '4/4组', dot: true, userType: 3 },
				{ name: '伸展', num: '4/4组', dot: true, userType: -1 },
				{ name: '伸展', num: '4/4组', dot: true, userType: -1 },
				{ name: '伸展', num: '4/4组', dot: true, userType: -1 }
			],
			picList: [
				{
					pic: '/static/pic1.png'
				},
				{
					pic: '/static/pic2.png'
				},
				{
					pic: '/static/pic3.png'
				}
			],
			LvList: ['非常好', '好', '一般', '不好'],
			appetiteLv: -1,
			vitalityLv: -1
		};
	},
	methods: {
		bindPickerChange(e, type) {
			switch (type) {
				case 1:
					this.appetiteLv = e.detail.value;
					break;
				case 2:
					this.vitalityLv = e.detail.value;
					break;
				case 3:
					this.algorithm = e.detail.value;
					break;
			}
		},
		changeUserType(index, idx) {
			let newObj = this.list[index];
			newObj.userType = Number(idx) + 1;
			this.list.splice(index, 1, newObj);
		}
	}
};
</script>

<style lang="scss">
.courseSummaryView {
	padding-bottom: 50px;
	background-color: #fff;
	.descView {
		width: 100%;
		height: 40px;
		line-height: 40px;
		padding-left: 25rpx;
		color: #fff;
		font-size: 28rpx;
		position: relative;
		&::after {
			position: absolute;
			left: 28px;
			bottom: -20px;
			width: 0;
			height: 0;
			border-top: 10px solid #0081ff;
			border-right: 10px solid transparent;
			border-bottom: 10px solid transparent;
			border-left: 10px solid transparent;
			content: '';
		}
	}

	.topBox {
		background-color: #4c515f;
		padding: 45rpx 30rpx;
		color: #fff;
		.headBox {
			width: 130rpx;
			height: 130rpx;
			line-height: 130rpx;
			text-align: center;
			font-size: 32rpx;
			margin-right: 40rpx;
			border-radius: 50%;
			background-color: #26b9a0;
		}
		.rightBox {
			font-size: 28rpx;
			.rTop {
				margin-bottom: 30rpx;
				.Id {
					margin-left: 35rpx;
					font-size: 24rpx;
					background-color: rgba(255, 255, 255, 0.3);
					padding: 6rpx 15rpx;
					border-radius: 8rpx;
				}
			}
			.rBottom {
				.phone {
					padding-right: 40rpx;
				}
				view {
					text-align: center;
					min-width: 100rpx;
					border-right: 1rpx solid #fff;
					&:last-child {
						border: none;
					}
				}
			}
		}
	}

	.classInfo {
		padding: 25rpx 30rpx;
		margin-bottom: 15rpx;
		color: #000;
		.userType {
			image {
				width: 34rpx;
				height: 34rpx;
			}
		}
	}

	.classDetail {
		.titBox {
			padding-top: 14px;
			font-size: 24rpx;
			margin-bottom: 8px;
			.tit {
				font-size: 28rpx;
				color: #000;
				font-weight: 700;
				.desc {
					font-size: 24rpx;
					color: #999;
				}
			}
		}
		.item {
			padding-bottom: 14px;
			position: relative;
			padding-left: 10rpx;
			&::after {
				width: 10rpx;
				height: 100%;
				background-color: #4bd3c4;
				content: '';
				position: absolute;
				left: 0;
				top: 0;
			}
			&.itemyellow {
				&::after {
					background-color: #fdb146;
				}
			}
			&.itemblue {
				&::after {
					background-color: #4bd3c4;
				}
			}
			&.itempurple {
				&::after {
					background-color: #799bf1;
				}
			}
			.itemcontent {
				padding-right: 25rpx;
				height: 44px;
				background-color: #f1f1f1;
				.dot {
					width: 4px;
					height: 4px;
					border-radius: 50%;
					background-color: red;
				}
				image {
					width: 32rpx;
					height: 32rpx;
					margin-right: 14rpx;
				}
			}
		}
	}

	.userSave {
		margin-top: 30px;
		.tit {
			line-height: 20px;
		}
		.box { 
			padding: 30rpx;
			border-radius: 8rpx;
			.item {
				width: 40%;
				padding-left: 20rpx;
				height: 22px;
				position: relative;
				&::after {
					width: 10rpx;
					height: 100%;
					background-color: #1a8ee9;
					content: '';
					position: absolute;
					left: 0;
					top: 0;
				}
				input {
					width: 20%;
					flex: 1;
					border-bottom: 1rpx solid #ededed;
					text-align: center;
					font-size: 24rpx;
				}
				picker {
					position: absolute;
					left: 0;
					top: 0;
					width: 100%;
					height: 100%;
					opacity: 0;
				}
			}
		}
		.userDesc{
			textarea{
				width: calc(100% - 50rpx);
				margin: 0 auto;
				border-radius: 8rpx;
				font-size: 22rpx;
				padding: 15rpx;
				height: 60px;
				background-color: #f1f1f1;
			}
		}
	}
	
	.btn{
		width: 100%;
		height: 34px;
		line-height: 34px;
		position: fixed;
		left: 0;
		bottom: 0;
	}
}
</style>
