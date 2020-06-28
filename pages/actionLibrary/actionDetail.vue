<template>
	<!-- 动作详情 -->
	<view class="actionDetailView">
	<!-- 	<cu-custom bgColor="bg-myblack" :isBack="true">
			<block slot="content">动作详情</block>
			<block slot="right"><text @click="gotoAddAction" style="font-size: 30rpx; margin-right: 14px;">编辑</text></block>
		</cu-custom> -->
		<!-- <view class="topBox">
			<text @click="gotoAddAction" style="font-size: 30rpx; ">编辑</text>
		</view> -->
		<view class="bannerBox">
			<image v-if="actionDetail.banner.type === 1" :src="actionDetail.banner.src" mode="aspectFill"></image>
			<video v-if="actionDetail.banner.type === 2" :src="actionDetail.banner.src" controls></video>
		</view>

		<view class="titleBox bg-white">
			<text>{{ actionDetail.title }}</text>
			<view class="flex flex-wrap margin-top-sm">
				<view v-for="(item, index) in actionDetail.label" :key="index" class="label">{{ item }}</view>
			</view>
		</view>

		<view class="section bg-white">
			<view class="tit">动作参数</view>
			<view class="parametersBox flex flex-wrap">
				<view v-for="(item, index) in actionDetail.actionParameters" :key="index" class="item">
					<view>{{ item.name }}:{{ item.data }}</view>
				</view>
			</view>
		</view>

		<view class="section bg-white">
			<view class="tit">锻炼部位</view>
			<view class="parametersBox flex flex-wrap">
				<view class="item"><view>无</view></view>
			</view>
		</view>

		<view class="section bg-white">
			<view class="tit">动作参数</view>
			<view class="parametersBox flex flex-wrap">
				<view class="item"><view>无</view></view>
			</view>
		</view>
		
		<view  @click="gotoAddAction"   class="btn bg-blue">
			编辑
		</view>
	</view>
</template>

<script>
export default {
	data() {
		return {
			actionDetail: {
				banner: { type: 1, src: '' },
				title: '牛角包翻转',
				actionParameters: [
					{ name: '组数', data: '3组' },
					{ name: '每组次数', data: '3次' },
					{ name: '每组事件', data: '无' },
					{ name: '组间休息', data: '无' },
					{ name: '阻力', data: '无' },
					{ name: '器械', data: '无' }
				],
				exerciseArea: [{ name: '无' }],
				actionIntroduction: [{ name: '无' }],
				label: ['背部', '热身’']
			}
		};
	},
	methods: {
		gotoAddAction() {
			uni.navigateTo({
				url: '/pages/actionLibrary/addAction'
			});
		}
	}
};
</script>

<style lang="scss">
.actionDetailView {
	padding-bottom: 65px;
	.topBox{
		font-size: 34rpx;
		padding-right: 25rpx;
		background-color: #4c515f;
		line-height: 30px;
		color: #fff;
		text-align: right;
	}
	.bannerBox {
		width: 100%;
		height: 300rpx;
		background: #1a8ee9;
	}
	.titleBox {
		min-height: 200rpx;
		margin-bottom: 18rpx;
		padding: 30rpx;
		color: #000;
		font-weight: 700;
		font-size: 36rpx;
		.label {
			width: 160rpx;
			height: 56rpx;
			line-height: 56rpx;
			background-color: #f9f9f8;
			margin-bottom: 14rpx;
			font-size: 28rpx;
			color: #666;
			border-radius: 8rpx;
			text-align: center;
			margin-right: 30rpx;
		}
	}
	.section {
		padding: 30rpx;
		.tit {
			padding-bottom: 16rpx;
			border-bottom: 1rpx solid #ededed;
			margin-bottom: 14rpx;
			padding-left: 30rpx;
			position: relative;
			font-size: 32rpx;
			color: #000;
			font-weight: 700;
			&::after {
				width: 10rpx;
				height: 32rpx;
				background-color: #1a8ee9;
				position: absolute;
				left: 0;
				top: 0;
				top: 5rpx;
				content: '';
			}
		}
		.parametersBox {
			.item {
				width: 50%;
				text-align: left;
				height: 80rpx;
				position: relative;
				padding-left: 34rpx;
				color: #000;
				margin-bottom: 10rpx;
				& > view {
					width: 5em;
				}
				&::after {
					width: 14rpx;
					height: 14rpx;
					background-color: #cccccc;
					border-radius: 50%;
					content: '';
					position: absolute;
					left: 0;
					top: 10rpx;
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
