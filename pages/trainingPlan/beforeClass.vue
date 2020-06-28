<template>
	<view class="beforClassView">
		<view class="title">课前准备</view>
		<view class="item flex align-center justify-between">
			<view class="name">客户状态</view>
			<view class="picBox flex align-center">
				<image @click="changePicType(index)" v-if="item.show" v-for="(item, index) in picType" :key="index" :src="item.pic" mode="aspectFill"></image>
			</view>
		</view>
		<view class="item flex align-center justify-between">
			<view class="name">心率检测</view>
			<switch @change="gotoAddInformation" :checked="ckecked" />
		</view>
		<view v-if="ckecked" class="item flex align-center justify-between">
			<view class="name">训练强度算法</view>
			<view style="position: relative;">
				<input class="age  " style="text-align: right;" type="text" :disabled="true" placeholder="体重" :value="algorithm > -1 ? algorithmList[algorithm] : ''" />
				<picker
					style="width: 100%;height: 100%;opacity: 0;position: absolute;left: 0;top: 0;"
					mode="selector"
					:range="algorithmList"
					@change="bindPickerChange($event, 3)"
				></picker>
			</view>
		</view>
		<view class="userType">
			<view class="tit">客户状态</view>
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
		</view>
		<view @click="startClass" class="btn cu-btn bg-blue">
			开始上课
		</view>
	</view>
</template>

<script>
export default {
	data() {
		return {
			LvList: ['非常好', '好', '一般', '不好'],
			appetiteLv: -1,
			vitalityLv: -1,
			picType: [
				{
					pic: '/static/pic1.png',
					show: true
				},
				{
					pic: '/static/pic2.png',
					show: true
				},
				{
					pic: '/static/pic3.png',
					show: true
				},
				{
					pic: '/static/pic4.png',
					show: true
				},
				{
					pic: '/static/pic5.png',
					show: true
				}
			],
			ckecked:false,
			algorithmList: ['最大心率算法', '最大心率算法2'],
			algorithm: 0
		};
	},
	methods: {
		startClass(){
			uni.navigateTo({
				url:'/pages/trainingPlan/startClass'
			})
		},
		gotoAddInformation(e) {
			if (e.detail.value) {
				uni.navigateTo({
					url: '/pages/trainingPlan/addInformation'
				});
				this.ckecked = true
			}else{
				this.ckecked = false
			}
		},
		changePicType(index) {
			let length = this.picType.filter(v => {
				return v.show === true;
			}).length;
			if (length > 1) {
				let list = this.picType;
				for (let idx in list) {
					list[idx].show = false;
				}
				list[index].show = true;
				this.picType = list;
			} else {
				let list = this.picType;
				for (let idx in list) {
					list[idx].show = true;
				}
				this.picType = list;
			}
		},
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
		}
	}
};
</script>

<style lang="scss">
page {
}
.beforClassView {
	padding: 0 30rpx;
	color: #000;
	background-color: #fff;
	min-height: 100vh;
	& > .title {
		line-height: 80px;
		font-size: 28rpx;
		text-align: center;
	}
	& > .item {
		height: 55px;
		border-bottom: 1rpx solid #ededed;
		.picBox {
			image {
				width: 50rpx;
				height: 50rpx;
				margin-right: 14rpx;
			}
		}
	}
	.userType {
		margin-top: 30px;
		.tit {
			line-height: 20px;
		}
		.box {
			background-color: #f6f6f6;
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
	}
	.btn{
		width: 100%;
		position: fixed;
		left: 50%;
		transform: translate(-50%);
		bottom: 0px;
		height: 60px;
		line-height: 60px;
		text-align: center; 
		margin: 0 auto;
	}
}
</style>
