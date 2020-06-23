<template>
	<!-- 首页 -->
	<view class="indexView" >
		<!-- <cu-custom bgImage="https://image.weilanwl.com/color2.0/plugin/cjkz2329.jpg">
			<block slot="content"><image src="/static/cjkz.png" mode="aspectFill" style="width: 240upx;height: 60upx;"></image></block>
		</cu-custom>

		<cu-custom bgImage="https://image.weilanwl.com/color2.0/plugin/sylb2244.jpg" :isBack="true">
			<block slot="backText">返回</block>
			<block slot="content">索引</block>
		</cu-custom> -->

		<!-- <cu-custom bgColor="bg-gradual-pink" :isBack="true"><block slot="backText">返回</block><block slot="content">导航栏</block></cu-custom> -->
		
		<view class="topBox" :style="[{ 'padding-top': CustomBar + 'px' }]" >
			<view class="userBox flex align-center  ">
				<image src="/static/logo.png" mode="aspectFill"></image>
				<text class="name">张三</text>
				<view @click="navgater('/pages/index/changeStore')"  class="changeStore flex align-center ">
					未来路店
					<text style="font-size: 60rpx;" class="cuIcon cuIcon-triangledownfill"></text>
				</view>
			</view>
			<view class="dataListBox flex">
				<view class="item bg-white">
					<view class="flex align-center justify-between">
						<view class="iconBox bg-cyan  flex align-center justify-center"><image src="/static/logo.png" mode="widthFix"></image></view>
						<view class="dataBox">
							<text>10000</text>
							<text class="desc">元</text>
						</view>
					</view>
					<view class="name">销售简报</view>
				</view>
				<view @click="navgater('/pages/index/monthData')" class="item bg-white">
					<view class="flex align-center justify-between">
						<view class="iconBox bg-mauve   flex align-center justify-center"><image src="/static/logo.png" mode="widthFix"></image></view>
						<view class="dataBox">
							<text>50</text>
							<text class="desc">节</text>
						</view>
					</view>
					<view class="name">本月上课</view>
				</view>
			</view>
		</view>

		<!-- 中间菜单 -->
		<view class="menuListBox bg-white grid text-center   col-4">
			<view @click="navgater(item.path)" v-for="(item, index) in menuList" :key="index" class="item flex flex-direction align-center ">
				<view :style="[{ 'background-color': item.color }]" class="iconBox flex justify-center align-center "><image :src="item.icon" mode="widthFix"></image></view>
				<text class="name">{{ item.name }}</text>
			</view>
		</view>

		<!-- 课程安排 -->
		<view class="curriculumListBox bg-white">
			<view class="title ">课程安排</view>
			<view v-for="(item, index) in curriculumList" :key="index" class="item  flex align-center justify-between">
				<view class="id">{{ item.id }}</view>
				<view class="name">{{ item.name }}</view>
				<view class="date">{{ item.date }}</view>
			</view>
		</view>
	</view>
</template>

<script>
export default {
	data() {
		return {
			CustomBar:this.CustomBar,
			menuList: [
				{
					icon: '/static/logo.png',
					name: '训练计划',
					color: '#ef750f'
				},
				{
					icon: '/static/logo.png',
					name: '训练方案',
					color: '#26b9a0'
				},
				{
					icon: '/static/logo.png',
					name: '训练模板',
					color: '#17b1c4',
					path:'/pages/trainingTemplate/trainingTemplate'
				},
				{
					icon: '/static/logo.png',
					name: '动作库',
					color: '#7570fe',
					path:'/pages/actionLibrary/actionLibrary'
				},
				{
					icon: '/static/logo.png',
					name: '工作统计',
					color: '#f6a713'
				},
				{
					icon: '/static/logo.png',
					name: '会员数据',
					color: '#9b71b7'
				},
				{
					icon: '/static/logo.png',
					name: '待续课会员',
					color: '#198aef'
				}
			],
			curriculumList:new Array(5).fill({id:'1234567890',name:'张三',date:'2020-06-20 17:20-18:20'})
		};
	},
	onLoad() {},
	methods: {
		navgater(path){
			uni.navigateTo({
				url:path
			})
		}
	}
};
</script>

<style lang="scss">
.indexView {
	padding-bottom: 20px;
	.topBox {
		padding: 0 30rpx 14rpx 30rpx;
		background-color: #4c515f;
		.userBox {
			color: #fff;
			font-size: 32rpx;
			& > image {
				width: 80rpx;
				height: 80rpx;
				border-radius: 50%;
				margin-right: 25rpx;
			}
			.changeStore {
				font-size: 26rpx;
				margin-left: 25rpx;
				background-color: rgba(0, 0, 0, 0.2);
				height: 44rpx;
				border-radius: 22rpx;
				padding-left: 22rpx;
				& > text {
					margin-left: 10rpx;
				}
			}
		}
		.dataListBox {
			margin-top: 20rpx;
			.item {
				border-radius: 14rpx;
				margin-right: 20px;
				padding: 25rpx;
				width: calc(50% - 10px);
				&:last-child {
					margin: 0;
				}
				.iconBox {
					width: 76rpx;
					height: 76rpx;
					border-radius: 50%;
					overflow: hidden;
					& > image {
						width: 42rpx;
					}
				}
				.dataBox {
					font-size: 38rpx;
					color: #000;
					.desc {
						font-size: 26rpx;
						color: #999;
						margin-left: 6rpx;
					}
				}
				.name {
					font-size: 34rpx;
					color: #000;
					margin-top: 20rpx;
				}
			}
		}
	}
	.menuListBox {
		padding-bottom: 90rpx;
		margin-bottom: 30rpx;
		.item {
			padding-top: 38rpx;
			.iconBox {
				background-color: red;
				width: 80rpx;
				height: 80rpx;
				border-radius: 12rpx;
				& > image {
					width: 60%;
				}
			}
			.name {
				font-size: 24rpx;
				color: #666;
				margin-top: 16rpx;
			}
		}
	}
	.curriculumListBox {
		.title {
			padding: 0 30rpx;
			font-size: 38rpx;
			color: #000;
			font-weight: 700;
			line-height: 2em;
			border-bottom: 1rpx solid #ededed;
		}
		.item {
			padding: 0 30rpx;
			line-height: 40px;
			font-size: 28rpx;
			border-bottom: 1rpx solid #ededed;
			&:last-child {
				border-bottom: none;
			}
			.id {
				width: 30%;
			}
			.data {
				flex: 1;
			}
		}
	}
}
</style>
