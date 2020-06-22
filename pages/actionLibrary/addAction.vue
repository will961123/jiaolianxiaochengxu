<template>
	<view class="addActionView">
		<view class="section bg-white">
			<view class="item flex align-center">
				<view class="name">名称</view>
				<input type="text" value="请输入动作名称" />
			</view>
		</view>

		<view class="section bg-white">
			<view class="title">动作参数</view>
			<view class="item flex align-center">
				<view class="name">组数</view>
				<picker @change="bindPickerChange($event, 1)" :range="groupsNumList">
					<text v-if="formData.groupsNum < 0" class=" text-gray">请选择</text>
					<text v-else style="line-height:60px ;" class="text-black">{{ groupsNumList[formData.groupsNum] }}</text>
					<text class="cuIcon  cuIcon-right text-gray"></text>
				</picker>
			</view>
			<view class="item flex align-center">
				<view class="name">每组次数</view>
				<picker @change="bindPickerChange($event, 2)" :range="oneGroupNumList">
					<text v-if="formData.oneGroupNum < 0" class=" text-gray">请选择</text>
					<text v-else style="line-height:60px ;" class="text-black">{{ oneGroupNumList[formData.oneGroupNum] }}</text>
					<text class="cuIcon  cuIcon-right text-gray"></text>
				</picker>
			</view>
			<view class="item flex align-center">
				<view class="name">每组时间</view>
				<picker @change="bindPickerChange($event, 3)" :range="oneGroupTimeList">
					<text v-if="formData.oneGroupTime < 0" class=" text-gray">请选择</text>
					<text v-else style="line-height:60px ;" class="text-black">{{ oneGroupTimeList[formData.oneGroupTime] }}</text>
					<text class="cuIcon  cuIcon-right text-gray"></text>
				</picker>
			</view>
			<view class="item flex align-center justify-between">
				<view class="name">
					可选器械
					<text class="text-gray margin-left-sm">(可多选)</text>
				</view>
				<view class="right text-right " style="width: 40%;">
					<text class=" text-gray">请选择</text>
					<text class="cuIcon  cuIcon-right text-gray"></text>
				</view>
			</view>
			<view class="item flex align-center justify-between">
				<view class="name">阻力</view>
				<view class="right text-right " style="width: 40%;">
					<text class=" text-gray">请选择</text>
					<text class="cuIcon  cuIcon-right text-gray"></text>
				</view>
			</view>
			<view class="item flex align-center">
				<view class="name">组间休息</view>
				<picker @change="bindPickerChange($event, 4)" :range="oneGroupRestTimeList">
					<text v-if="formData.oneGroupRestTime < 0" class=" text-gray">请选择</text>
					<text v-else style="line-height:60px ;" class="text-black">{{ oneGroupRestTimeList[formData.oneGroupRestTime] }}</text>
					<text class="cuIcon  cuIcon-right text-gray"></text>
				</picker>
			</view>
		</view>

		<view class="section bg-white">
			<view class="title">动作设置</view>
			<view class="item flex align-center">
				<view class="name">部位</view>
				<picker @change="bindPickerChange($event, 5)" :range="positionList">
					<text v-if="formData.position < 0" class=" text-gray">请选择</text>
					<text v-else style="line-height:60px ;" class="text-black">{{ positionList[formData.position] }}</text>
					<text class="cuIcon  cuIcon-right text-gray"></text>
				</picker>
			</view>
			<view class="item flex align-center justify-between">
				<view class="name">标签</view>
				<view class="right text-right " style="width: 40%;">
					<text class=" text-gray">请选择</text>
					<text class="cuIcon  cuIcon-right text-gray"></text>
				</view>
			</view>
			<view class="item flex align-center justify-between">
				<view class="name">共享设置</view>
				<view class="right text-right " style="width: 40%;">
					<text class=" text-gray">请选择</text>
					<text class="cuIcon  cuIcon-right text-gray"></text>
				</view>
			</view>
		</view>

		<view class="section bg-white">
			<view class="title">动作详情</view>
			<view class="item flex align-center">
				<view class="name">部位</view>
				<picker @change="bindPickerChange($event, 5)" :range="positionList">
					<text v-if="formData.position < 0" class=" text-gray">请选择</text>
					<text v-else style="line-height:60px ;" class="text-black">{{ positionList[formData.position] }}</text>
					<text class="cuIcon  cuIcon-right text-gray"></text>
				</picker>
			</view>
			<view class="item flex align-center justify-between">
				<view class="name">动作视频/图片</view>
				<view class="right text-right " style="width: 40%;">
					<text class=" text-gray">请选择</text>
					<text class="cuIcon  cuIcon-right text-gray"></text>
				</view>
			</view>
			<view class="item flex align-center justify-between">
				<view class="name">动作要领</view>
				<view class="right text-right " style="width: 40%;">
					<text class=" text-gray">请选择</text>
					<text class="cuIcon  cuIcon-right text-gray"></text>
				</view>
			</view>
		</view>

		<button class="btn cu-btn bg-blue">完成</button>
	</view>
</template>

<script>
export default {
	data() {
		return {
			groupsNumList: [], // 组数
			oneGroupNumList: [], // 每组次数
			oneGroupTimeList: [], // 每组时间
			oneGroupRestTimeList: [], // 每组休息
			positionList: ['全部', '胸部', '背部', '肩部', '手臂', '颈部', '腹部', '腰部', '臀部', '腿部', '全身', '躯干'], //部位

			formData: {
				groupsNum: 2,
				oneGroupNum: 2,
				oneGroupTime: -1,
				oneGroupRestTime: -1,
				position: -1
			}
		};
	},
	onLoad() {
		for (var i = 0; i < 100; i++) {
			this.groupsNumList.push(i + 1 + '组');
			this.oneGroupNumList.push(i + 1 + '次');
			this.oneGroupTimeList.push(i + 1 + '秒');
		}
		for (var i = 0; i < 600; i++) {
			this.oneGroupRestTimeList.push(i + '秒');
		}
	},
	methods: {
		bindPickerChange(e, type) {
			switch (type) {
				case 1:
					this.formData.groupsNum = e.detail.value;
					console.log(e.detail.value, this.formData.groupsNum);
					break;
				case 2:
					this.formData.oneGroupNum = e.detail.value;
					break;
				case 3:
					this.formData.oneGroupTime = e.detail.value;
					break;
				case 4:
					this.formData.oneGroupRestTime = e.detail.value;
					break;
				case 5:
					this.formData.position = e.detail.value;
					break;
			}
		}
	}
};
</script>

<style lang="scss">
.addActionView {
	padding-bottom: 44px;
	.section {
		.item {
			padding: 0 30rpx;
			height: 60px;
			font-size: 28rpx;
			border-bottom: 1rpx solid #ededed;
			&:last-child {
				border-bottom: none;
			}
			.name {
				color: #000;
			}
			input,
			picker {
				font-size: 28rpx;
				height: 100%;
				flex: 1;
				text-align: right;
			}
			picker {
				line-height: 60px;
			}
		}
		.title {
			padding: 0 30rpx;
			line-height: 42px;
			height: 42px;
			background-color: #efefef;
			color: #b1b1b1;
			font-size: 28rpx;
		}
	}
	.btn {
		border-radius: 0;
		width: 100%;
		height: 44px;
		position: fixed;
		left: 0;
		bottom: 0;
	}
}
</style>
