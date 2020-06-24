<template>
	<!-- 补充信息 -->
	<view class="addInformationView">
		<view class="desc">
			请帮助完善客户的基本信息
			<br />
			有助于得到更准确的运动数据
		</view>
		<view class="tit">年龄</view>
		<view style="position: relative;">
			<input class="age  " type="text" :disabled="true" placeholder="年龄" :value="age > -1 ? ageList[age] : ''" />
			<picker style="width: 100%;height: 60px;opacity: 0;position: absolute;left: 0;top: 20px;z-index: 99;" mode="selector" :range="ageList" @change="bindPickerChange($event, 1)">请选择</picker>
		</view>

		<view class="tit">体重</view>
		<view style="position: relative;">
			<input class="age  " type="text" :disabled="true" placeholder="体重" :value="weight > -1 ? weightList[weight] : ''" />
			<picker
				style="width: 100%;height: 60px;opacity: 0;position: absolute;left: 0;top: 20px;z-index: 99;"
				mode="selector"
				:range="weightList"
				@change="bindPickerChange($event, 2)"
			>请选择</picker>
		</view>
		
		<view @click="saveInformation" class="btn cu-btn bg-blue">
			下一步
		</view>
	</view>
</template>

<script>
export default {
	data() {
		return {
			ageList: [],
			age: -1,
			weightList: [],
			weight: -1
		};
	},
	onLoad() {
		for (var i = 10; i < 90; i++) {
			this.ageList.push(i + '岁');
		}
		for (var i = 40; i < 150; i += 0.5) {
			this.weightList.push(i + 'kg');
		}
	},
	methods: {
		saveInformation(){
			uni.navigateBack({
				delta:1
			})
		},
		bindPickerChange(e, type) {
			switch (type) {
				case 1:
					this.age = e.detail.value;
					break;
				case 2:
					this.weight = e.detail.value;
					break;
			}
		}
	}
};
</script>

<style lang="scss">
page {
	background-color: #4d505f;
}
.addInformationView {
	text-align: center;
	padding-top: 20px;
	.desc {
		color: #fff;
		font-size: 28rpx;
	}
	& > .tit {
		color: #fff;
		font-size: 28rpx;
		margin-top: 30px;
		margin-bottom: 20px;
	}
	.age {
		margin: 0 auto;
		width: 100px;
		height: 50px;
		border-bottom: 1rpx solid #585b6a;
		color: #56b3ab;
	}
	.btn{
		width: 85%;
		position: fixed;
		left: 50%;
		transform: translate(-50%);
		bottom: 10px;
		height: 40px;
		line-height: 40px;
		text-align: center;
		border-radius: 20px;
		margin: 0 auto;
	}
}
</style>
