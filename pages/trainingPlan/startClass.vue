<template>
	<view class="startClassView">
		<view class="topBox  ">
			<view class="menuBox flex align-center justify-end">
				<text class="margin-right-lg">{{ fillterTime }}</text>
				<button class="btn cu-btn bg-blue sm margin-right-lg">关联预约记录</button>
				<button @click="stop" class="btn cu-btn bg-red sm margin-right-lg">结束</button>
			</view>
			<view class="userBox flex">
				<view class="headBox"><image src="/static/logo.png" mode="aspectFill"></image></view>
				<view class="right flex flex-direction justify-between">
					<view class="rTop flex justify-between">
						<view class="name">张三</view>
						<view class="phone">
							<text class="cuIcon cuIcon-profile"></text>
							1599999999
						</view>
						<view class="id">
							<text class="cuIcon cuIcon-phone"></text>
							15999999999
						</view>
					</view>
					<view class="rBottom flex  align-center">
						<view class="name margin-right-lg">
							<text class="cuIcon cuIcon-text"></text>
							减脂课
						</view>
						<view class="day">
							<text class="cuIcon cuIcon-edit"></text>
							2020-06-24 16:17--17:10
						</view>
						<!-- <view class="time">
							<text class="cuIcon cuIcon-time"></text>
							
						</view> -->
					</view>
				</view>
			</view>
		</view>

		<view class="progressBox bg-white flex">
			<view v-for="(item, index) in 5" :key="index" class="lineBox flex ">
				<view @click="changeNowIdx(index)" class="item flex flex-direction align-end">
					<view :class="index <= nowIdx ? 'select' : ''" class="line"></view>
					<view class="desc">
						<text class="name">{{ index }}伸展</text>
						<view class="num">4组</view>
					</view>
				</view>
			</view>
		</view>

		<view class="tableTh flex justify-between align-center bg-white">
			<view class="left flex">
				<view class="item">数量</view>
				<view class="item">重量</view>
				<view class="item">休息</view>
				<view class="item">器械</view>
			</view>
			<view class="right">完成情况</view>
		</view>
		
		
		<swiper @change="swiperChange" :current="nowIdx" :style="{ height: scrollViewHeight }"  >
			<swiper-item v-for="(switem,swidx) in 5" :key="swidx" >
				 <scroll-view class="bg-white" :style="{ height: scrollViewHeight }" id="scrollView" scroll-y="true">
				 	<view class="cu-list menu-avatar parameterBox">
				 		<view
				 			class="cu-item item "
				 			:class="modalName == 'move-box-' + idx ? 'move-cur' : ''"
				 			v-for="(itm, idx) in list"
				 			:key="idx"
				 			@touchstart="ListTouchStart"
				 			@touchmove="ListTouchMove"
				 			@touchend="ListTouchEnd"
				 			:data-target="'move-box-' + idx"
				 		>
				 			<picker
				 				style="display: inline-block;width: 18%;text-align: center;border-right: 1rpx solid #dddddd;"
				 				:range="numList"
				 				@change="bindPickerChange($event, 1, idx)"
				 				mode="multiSelector"
				 			>
				 				{{ itm.num }}{{ numList[1][itm.numType] }}
				 			</picker>
				 			<text @tap="showModal" :data-changeidx="idx" data-target="Resistance">{{ itm.kg }}{{ itm.kgType }}</text>
				 			<text @tap="showModal" :data-changeidx="idx" data-target="Rest" Rest class="no-border">休息{{ itm.rest }}s</text>
				 			<text @tap="showModal" style="border: none;" :data-changeIdx="idx" data-target="ChooseModal">{{ itm.equipment }}</text>
				 			<view class="picBox flex align-center justify-end">
				 				<block v-if="itm.doType === -1">
				 					<image @click="changePicType(idx, index)" v-for="(item, index) in picType" :key="index" :src="item.pic" mode="aspectFill"></image>
				 				</block>
				 
				 				<image v-else :src="'/static/pic' + itm.doType + '.png'" mode="aspectFill"></image>
				 			</view>
				 			<view @click.stop="del(idx)" class="move"><view class="bg-red">删除</view></view>
				 		</view>
				 
				 		<view @click="addOneItem" class=" item addItem flex align-center justify-center">
				 			<view style="font-weight: 700;margin-right: 10rpx;" class="text-black ">+</view>
				 			<view>添加组</view>
				 		</view>
				 	</view>
				 </scroll-view>
			</swiper-item>  
		</swiper> 

		<view class="bottomMenuBox">
			<view class="addBox">
				<view @click="ChooseImage" class="pic flex justify-center align-center"><text class="cuIcon cuIcon-cameraaddfill"></text></view>
			</view>
		</view>

		<!-- 可选器械 -->
		<view class="cu-modal bottom-modal " :class="modalName == 'ChooseModal' ? 'show' : ''" @tap="hideModal">
			<view class="cu-dialog bg-white" @tap.stop="">
				<view class="cu-bar flex justify-end">
					<!-- <view class="action text-blue" @tap="hideModal">取消</view> -->
					<view class="action text-green " @tap="hideModal">确定</view>
				</view>
				<view style="height: 40vh;overflow-y: auto;position: relative;">
					<view v-for="(group, index) in apparatus" :key="index">
						<view class="groupTit margin-bottom-xl">{{ group.title }}</view>
						<view class="flex flex-wrap  ">
							<view
								@click="selectOne(index, idx, item.id)"
								v-for="(item, idx) in group.list"
								class="groupItem flex1 padding-xs margin-bottom-sm "
								:class="apparatusSelectIdx[0] == index && apparatusSelectIdx[1] == idx ? 'bg-blue' : ''"
								:key="idx"
							>
								{{ item.name }}
							</view>
						</view>
					</view>
				</view>
				<view v-show="showAddIpt" class="iptbox">
					<view class="ipt bg-white flex align-center justify-between">
						<input v-model="addApparatuStr" type="text" placeholder="请输入" />
						<view @click="saveNewAddStr" class="text-blue">确定</view>
					</view>
				</view>
			</view>
		</view>

		<!-- 阻力-->
		<view class="cu-modal bottom-modal " :class="modalName == 'Resistance' ? 'show' : ''" @tap="hideModal">
			<view style="padding: 0;" class="cu-dialog bg-white" @tap.stop="">
				<view class="iptbox2">
					<view class="resistanceTypeList flex">
						<view
							@click.stop="chnageResistanceType(index)"
							v-for="(item, index) in resistanceTypeList"
							:key="index"
							:class="index === resistanceType ? 'item select' : 'item'"
						>
							{{ item }}
						</view>
					</view>
					<view class="ipt bg-white flex align-center justify-between">
						<input :focus="focus" v-model="resistance" type="digit" placeholder="请输入" />
						<view @click="saveKgChange" class="text-blue">确定</view>
					</view>
				</view>
			</view>
		</view>

		<!-- 休息-->
		<view class="cu-modal bottom-modal " :class="modalName == 'Rest' ? 'show' : ''" @tap="hideModal">
			<view style="padding: 0;" class="cu-dialog bg-white" @tap.stop="">
				<view class="iptbox2">
					<view class="ipt bg-white flex align-center justify-between">
						<input :focus="focus2" v-model="restValue" type="number" placeholder="请输入" />
						<view @click="saveRestChange" class="text-blue">确定</view>
					</view>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
let timer = null;
export default {
	data() {
		return {
			focus: false,
			focus2: false,
			nowIdx: 2,
			time: 0,
			scrollViewHeight: 'calc(30vh)', // scrollview 高度
			modalName: '',
			list: [
				{ num: 3, numType: 0, equipment: '哑铃', kg: 1, kgType: 'KG', rest: -1, doType: -1 },
				{ num: 4, numType: 0, equipment: '哑铃', kg: 1, kgType: 'KG', rest: -1, doType: 1 },
				{ num: 5, numType: 0, equipment: '哑铃', kg: 1, kgType: 'KG', rest: -1, doType: -1 },
				{ num: 3, numType: 0, equipment: '哑铃', kg: 1, kgType: 'KG', rest: -1, doType: -1 },
				{ num: 4, numType: 0, equipment: '哑铃', kg: 1, kgType: 'KG', rest: -1, doType: 1 },
				{ num: 5, numType: 0, equipment: '哑铃', kg: 1, kgType: 'KG', rest: -1, doType: -1 }
			],
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
				}
			],
			type: '',
			numList: [[], ['次', '分', '秒']],

			// 这几个可选器械数据模型
			apparatus: [],
			apparatusSelectIdx: [-1, -1],
			showAddIpt: false,
			addApparatuStr: '',

			// 阻力数据模型
			resistance: '',
			resistanceType: 0,
			resistanceTypeList: ['KG', 'LBS', '级'],

			// 休息
			restValue: '',

			changeIdx: -1,

			imgList: []
		};
	},
	computed: {
		fillterTime() {
			let s = (this.time % 60) + '';
			let m = parseInt(this.time / 60) + '';
			s[1] ? '' : (s = '0' + s);
			m[1] ? '' : (m = '0' + m);
			return m + ':' + s;
		}
	},
	onReady() {
		const query = uni.createSelectorQuery().in(this);
		query
			.select('#scrollView')
			.boundingClientRect(data => {
				this.scrollViewHeight = `calc(100vh - ${data.top.toFixed(2)}px - 50px)`;
			})
			.exec();
	},
	onLoad() {
		timer = setInterval(() => {
			this.time++;
		}, 1000);
		for (var i = 0; i < 100; i++) {
			this.numList[0].push(1 + i);
		}
		this.apparatus = [
			{
				title: '徒手训练',
				list: [{ name: '徒手训练', id: 1 }]
			},
			{
				title: '自由器械',
				list: [
					{ name: '哑铃', id: 2 },
					{ name: '杠铃', id: 3 },
					{ name: '壶铃', id: 4 },
					{ name: '弹力带', id: 5 },
					{ name: 'TRX绳', id: 6 },
					{ name: '战绳', id: 7 },
					{ name: '药球', id: 8 },
					{ name: '瑞士球', id: 9 },
					{ name: '波速球', id: 10 },
					{ name: '泡沫轴', id: 11 }
				]
			},
			{
				title: '固定器械',
				list: [
					{ name: '龙门架', id: 12 },
					{ name: '史密斯机', id: 13 },
					{ name: '推胸机', id: 14 },
					{ name: '悍马机', id: 15 },
					{ name: '夹胸机', id: 16 },
					{ name: '划船机', id: 17 },
					{ name: '下拉机', id: 18 },
					{ name: '地震架', id: 19 },
					{ name: '推肩器', id: 20 },
					{ name: '倒蹬机', id: 21 },
					{ name: '哈克机', id: 22 },
					{ name: '腿弯举机', id: 23 },
					{ name: '腿屈伸机', id: 24 },
					{ name: '提踵机', id: 25 },
					{ name: '单杠', id: 26 },
					{ name: '双杠', id: 27 },
					{ name: '仰卧板', id: 28 },
					{ name: '罗马椅子', id: 29 },
					{ name: '牧师凳', id: 30 },
					{ name: '有氧器械', id: 31 }
				]
			},
			{
				title: '其他',
				list: [{ name: '自定义', id: -1 }]
			}
		];
	},
	onUnload() {
		clearInterval(timer);
	},
	methods: {
		swiperChange(e){
			console.log(e)
			if(e.detail.source === 'touch'){
				this.nowIdx = e.detail.current
			}
		},
		addOneItem() {
			this.list.push(this.list[0]);
		},
		changeNowIdx(idx) {
			this.nowIdx = idx;
		},
		stop() {
			uni.navigateTo({
				url: '/pages/trainingPlan/courseSummary'
			});
		},
		chnageResistanceType(type) {
			this.resistanceType = type;
			this.focus = false;
			setTimeout(()=>{  
				this.focus = true;
			},500) 
		},
		ChooseImage() {
			uni.chooseImage({
				count: 1, //默认9
				sizeType: ['original', 'compressed'], //可以指定是原图还是压缩图，默认二者都有
				sourceType: ['album'], //从相册选择
				success: res => {
					if (this.imgList.length != 0) {
						this.imgList = this.imgList.concat(res.tempFilePaths);
					} else {
						this.imgList = res.tempFilePaths;
					}
					// this.showLoading();
					// let url = this.uploadUrl + '/rentSaleHouse/uploadRentPic';
					// if (this.title === '我要卖房') {
					// 	url = this.uploadUrl + '/rentSaleHouse/uploadSalePic';
					// } else if (this.title !== '我要卖房' && this.title !== '我要出租') {
					// 	url = this.uploadUrl + '/rentSaleShop/upload';
					// }
					// uni.uploadFile({
					// 	url: url,
					// 	filePath: res.tempFilePaths[0],
					// 	name: 'files',
					// 	formData: {},
					// 	success: res => {
					// 		uni.hideLoading();
					// 		let resoult = JSON.parse(res.data);
					// 		console.log('上传图片', resoult);
					// 		if (resoult.code === 200) {
					// 			this.imgSrcList.push(...resoult.data);
					// 			console.log(this.imgSrcList);
					// 		} else {
					// 			this.showToast(resoult.info);
					// 		}
					// 	},
					// 	fail: err => {
					// 		console.log(err);
					// 	}
					// });
				}
			});
		},
		saveNewAddStr() {
			this.showAddIpt = false;
			if (this.addApparatuStr) {
				let newSelectObj = {
					name: this.addApparatuStr,
					id: new Date().getTime()
				};
				this.addApparatuStr = '';
				this.apparatus[this.apparatus.length - 1].list.unshift(newSelectObj);
			}
		},
		selectOne(index, idx, id) {
			if (id === -1) {
				this.showAddIpt = true;
				return false;
			}

			this.apparatusSelectIdx[0] = index;
			this.apparatusSelectIdx[1] = idx;
			let newObj = this.list[this.changeIdx];
			newObj.equipment = this.apparatus[index].list[idx].name;
			this.list.splice(this.changeIdx, 1, newObj);

			// this.hideModal();
		},
		// 间休
		saveRestChange() {
			if (!this.restValue) {
				return;
			}
			let newObj = this.list[this.changeIdx];
			newObj.rest = this.restValue;
			this.list.splice(this.changeIdx, 1, newObj);
			this.restValue = '';

			this.hideModal();
		},
		// 阻力
		saveKgChange() {
			let newObj = this.list[this.changeIdx];
			newObj.kg = this.resistance;
			newObj.kgType = this.resistanceTypeList[this.resistanceType];
			this.list.splice(this.changeIdx, 1, newObj);
			this.hideModal();
		},
		showModal(e) {
			this.modalName = e.currentTarget.dataset.target;
			this.changeIdx = e.currentTarget.dataset.changeidx;
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
		},
		bindPickerChange(e, type, idx) {
			switch (type) {
				case 1:
					let newObj = this.list[idx];
					newObj.num = e.detail.value[0] + 1;
					newObj.numType = e.detail.value[1];
					this.list.splice(idx, 1, newObj);
					break;
			}
		},
		changePicType(itemIdx, picIdx) {
			let newObj = this.list[itemIdx];
			newObj.doType = picIdx + 1;
			this.list.splice(itemIdx, 1, newObj);
		},
		del(idx) {
			uni.showModal({
				title: '删除动作',
				content: `确定删除吗?`,
				success: res => {
					if (res.confirm) {
						this.list.splice(idx, 1);
					}
				}
			});
		},
		// ListTouch触摸开始
		ListTouchStart(e) {
			this.listTouchStart = e.touches[0].pageX;
		},
		// ListTouch计算方向
		ListTouchMove(e) {
			this.isClick = false;
			console.log(e.touches[0].pageX - this.listTouchStart);
			if (Math.abs(e.touches[0].pageX - this.listTouchStart) < 20) {
				return;
			}
			this.listTouchDirection = e.touches[0].pageX - this.listTouchStart > 0 ? 'right' : 'left';
		},
		// ListTouch计算滚动
		ListTouchEnd(e) {
			if (this.listTouchDirection == 'left') {
				this.modalName = e.currentTarget.dataset.target;
			} else {
				this.modalName = null;
			}
			this.listTouchDirection = null;
		}
	}
};
</script>

<style lang="scss">
.startClassView {
	.topBox {
		color: #fff;
		background-color: #4c515f;
		padding-bottom: 20px;
		.userBox {
			padding: 30rpx;
			.headBox {
				width: 110rpx;
				height: 110rpx;
				margin-right: 30rpx;
				& > image {
					width: 100%;
					height: 100%;
					border-radius: 50%;
				}
			}
			.right {
				padding: 4rpx 0;
				flex: 1;
			}
		}
	}
	.progressBox {
		width: 100%;
		padding-top: 20px;
		margin-top: 14px;
		overflow-x: auto;
		padding-bottom: 8px;
		.lineBox {
			.item {
				.line {
					width: 150px;
					height: 4px;
					background-color: #f1f1f1;
					position: relative;
					&::after {
						width: 12px;
						height: 12px;
						border-radius: 50%;
						right: 20px;
						top: -4px;
						content: '';
						position: absolute;
						background-color: #999;
					}
					&.select {
						background-color: #1d90ff;
						&::after {
							background-color: #1d90ff;
						}
					}
				}
				.desc {
					margin-top: 8px;
					text-align: center;
					.name {
						font-size: 34rpx;
					}
				}
			}
		}
	}
	.tableTh {
		border-top: 1rpx solid #ededed;
		height: 44px;
		.left {
			flex: 1;
			& > view {
				flex: 1;
				text-align: center;
			}
		}
		.right {
			width: 25%;
			text-align: center;
		}
	}

	.parameterBox {
		padding-left: 30rpx;
		// padding-right: 30rpx;
		border-radius: 8rpx;
		// margin-top: 14px;
		padding-top: 20px;

		.item {
			height: 54px;
			position: relative;
			margin-bottom: 20px;
			background-color: #f6f6f6;

			// &::after {
			// 	width: 10rpx;
			// 	height: 10rpx;
			// 	border-radius: 50%;
			// 	position: absolute;
			// 	top: 0;
			// 	left: 0;
			// 	margin-top: 14rpx;
			// 	content: '';
			// 	background-color: #8695a4;
			// }
			& > text {
				// color: #333333;
				font-size-adjust: 22rpx;
				display: inline-block;
				width: 18%;
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
		.addItem {
			border: 1rpx dashed #666;
			background-color: #fff;
			text-align: center;
		}
	}
	.picBox {
		flex: 1;
		image {
			width: 40rpx;
			height: 40rpx;
			margin-right: 14rpx;
		}
	}

	.cu-modal {
		z-index: 999 !important;
		.cu-dialog {
			padding: 0 25rpx;
			padding-bottom: 14px;
			.groupTit {
				margin-top: 16px;
				font-size: 30rpx;
				color: #000;
				font-weight: 700;
				text-align: left;
			}
			.groupItem {
				width: 17%;
				margin-right: 3%;
				background-color: #efefef;
				border-radius: 8rpx;
				font-size: 24rpx;
				&:nth-child(5n) {
					margin-right: 0;
				}
				&.bg-blue {
					background-color: #0081ff;
				}
			}
			.iptbox {
				position: absolute;
				left: 0;
				bottom: 0;
				z-index: 1000;
				width: 100%;
				height: 100%;
				background-color: rgba(0, 0, 0, 0.6);
				.ipt {
					padding: 0 25rpx;
					width: 100%;
					height: 54px;
					position: absolute;
					left: 0;
					bottom: 0;
					input {
						height: 100%;
						flex: 1;
						text-align: left;
						font-size: 28rpx;
					}
				}
			}
			.iptbox2 {
				.ipt {
					padding: 0 25rpx;
					width: 100%;
					height: 56px;
					input {
						height: 100%;
						flex: 1;
						text-align: left;
						font-size: 28rpx;
					}
				}
				.resistanceTypeList {
					padding-bottom: 2px;
					background-color: #efefef;
					.item {
						transition: background-color 0.5s;
						height: 40px;
						line-height: 40px;
						flex: 1;
						border-right: 1rpx solid #999;
						&:last-child {
							border: none;
						}
						&.select {
							border-radius: 6px;
							background-color: #fff;
							border: none;
						}
					}
				}
			}
			.labelItem {
				padding: 0 30rpx;
				line-height: 46px;
				border-bottom: 1rpx solid #ededed;
				radio {
					transform: scale(0.8);
				}
			}
		}
	}
	.bottomMenuBox {
		height: 50px;
		width: 100%;
		background-color: #4d505f;
		.addBox {
			padding: 15rpx 25rpx;
			border-left: 1rpx solid #fff;
			float: right;
			.pic {
				width: 60rpx;
				height: 60rpx;
				color: #4d505f;
				background-color: #fff;
				border-radius: 50%;
				font-size: 34rpx;
			}
		}
	}
}
</style>
