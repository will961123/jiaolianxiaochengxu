<template>
	<!-- 添加训练课 -->
	<view class="addTrainingClassView">
		<!-- <cu-custom bgColor="bg-myblack" :isBack="true">
			<block slot="content">编辑课程</block>
			<block slot="right"><text style="margin-right: 25rpx;">保存</text></block>
		</cu-custom> -->
	<!-- 	<view class="topBox">
			<text  >保存</text>
		</view> -->
		<view class="title bg-white flex align-center"><input type="text" value="" placeholder="请输入训练主题" /></view>
		<view class="section bg-white">
			<view class="titBox flex justify-between align-center">
				<view class="flex align-center">
					<view style="background-color: #fdb146;" class="label "></view>
					<view class="tit">热身训练</view>
				</view>
			</view>

			<view v-for="(item, index) in detailInfo.warmUp" :key="index" class="listBox">
				<view @click="changeShowChilder" :data-idx="index" data-key="warmUp" class="list flex justify-between align-center">
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

					<view @click="changeShowChilder" :data-idx="index" data-key="warmUp" :class="item.select ? 'cuIcon-fold' : 'cuIcon-unfold'" class="cuIcon "></view>
				</view>
				<view v-show="item.select" class="parameterBox">
					<view v-for="(itm, idx) in item.list" :key="idx" class="item">
						<picker
							style="display: inline-block;width: 18%;text-align: center;border-right: 1rpx solid #dddddd;"
							:range="numList"
							@change="bindPickerChange($event, 1, 'warmUp', index, idx)"
							mode="multiSelector"
						>
							{{ itm.num }}{{ numList[1][itm.numType] }}
						</picker>
						<text @tap="showModal" data-key="warmUp" :data-idx1="index" :data-idx2="idx" data-target="ChooseModal">{{ itm.equipment }}</text>
						<text @tap="showModal" data-key="warmUp" :data-kgidx1="index" :data-kgidx2="idx" data-target="Resistance">{{ itm.kg }}{{ itm.kgType }}</text>
						<text @tap="showModal" data-key="warmUp" :data-restidx1="index" :data-restidx2="idx" data-target="Rest" Rest class="no-border">休息{{ itm.rest }}s</text>
						<view @click="delOneParamete(index, idx, 'warmUp')" class=" close cuIcon cuIcon-roundclose"></view>
					</view>
					<view @click="addOneItem" data-key="warmUp" :data-addidx1="index" style="text-align: left;" class="addBox cuIcon text-blue text-left">+ 添加</view>
				</view>
			</view>

			<view @click="gotoActionLibrary" class="addBox text-blue">
				<text class="cuIcon cuIcon-add"></text>
				添加动作
			</view>
		</view>

		<view class="section bg-white">
			<view class="titBox flex justify-between align-center">
				<view class="flex align-center">
					<view style="background-color: #4bd3c4;" class="label  "></view>
					<view class="tit">正式训练</view>
				</view>
			</view>
			<view @click="gotoActionLibrary" class="addBox text-blue">
				<text class="cuIcon cuIcon-add"></text>
				添加动作
			</view>
		</view>

		<view class="section bg-white">
			<view class="titBox flex justify-between align-center">
				<view class="flex align-center">
					<view style="background-color: #799bf1;" class="label  "></view>
					<view class="tit">放松整理</view>
				</view>
			</view>
			<view @click="gotoActionLibrary" class="addBox text-blue">
				<text class="cuIcon cuIcon-add"></text>
				添加动作
			</view>
		</view>
		
		<view   class="btn bg-blue">
			保存
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
						<view @click.stop="saveNewAddStr" class="text-blue">确定</view>
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
							@click.stop="changeResistanceType(index)"
							v-for="(item, index) in resistanceTypeList"
							:key="index"
							:class="index === resistanceType ? 'item select' : 'item'"
						>
							{{ item }}
						</view>
					</view>
					<view class="ipt bg-white flex align-center justify-between">
						<input :focus="focus" v-model="resistance" type="digit" placeholder="请输入" />
						<view @click.stop="saveKgChange" class="text-blue">确定</view>
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
						<view @click.stop="saveRestChange" class="text-blue">确定</view>
					</view>
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
			numList: [[], ['次', '分', '秒']],
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
			modalName: '',
			// 这几个可选器械数据模型
			equipmentKey: '',
			equipmentIdx1: '',
			equipmentIdx2: '',
			apparatus: [],
			apparatusSelectIdx: [-1, -1],
			showAddIpt: false,
			addApparatuStr: '',

			// 阻力数据模型
			kgKey: '',
			kgIdx1: '',
			kgIdx2: '',
			resistance: '',
			resistanceType: 0,
			resistanceTypeList: ['KG', 'LBS', '级'],

			// 休息
			restKey: '',
			restIdx1: '',
			restIdx2: '',
			restValue: ''
		};
	},
	onLoad() {
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
	methods: {
		changeShowChilder(e) {  
			this.detailInfo[e.currentTarget.dataset.key][e.currentTarget.dataset.idx].select = !this.detailInfo[e.currentTarget.dataset.key][e.currentTarget.dataset.idx].select;
		},
		changeResistanceType(type) {
			this.resistanceType = type; 
			this.focus = false;
			setTimeout(()=>{  
				this.focus = true;
			},500) 
		},
		// 添加一项
		addOneItem(e) {
			this.detailInfo[e.currentTarget.dataset.key][e.currentTarget.dataset.addidx1].list.push(
				this.detailInfo[e.currentTarget.dataset.key][e.currentTarget.dataset.addidx1].list[
					this.detailInfo[e.currentTarget.dataset.key][e.currentTarget.dataset.addidx1].list.length - 1
				]
			);
		},
		// 间休
		saveRestChange() {
			if (!this.restValue) {
				return;
			}
			let newObj = this.detailInfo[this.restKey][this.restIdx1].list[this.restIdx2];
			newObj.rest = this.restValue;
			this.detailInfo[this.restKey][this.restIdx1].list.splice(this.restIdx2, 1, newObj);
			this.restValue = '';

			this.hideModal();
		},
		// 阻力
		saveKgChange() {
			let newObj = this.detailInfo[this.kgKey][this.kgIdx1].list[this.kgIdx2];
			newObj.kg = this.resistance;
			newObj.kgType = this.resistanceTypeList[this.resistanceType];
			this.detailInfo[this.kgKey][this.kgIdx1].list.splice(this.kgIdx2, 1, newObj);

			this.hideModal();
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
		// 器材
		selectOne(index, idx, id) {
			if (id === -1) {
				this.showAddIpt = true;
				return false;
			}

			// 所有器材列表索引
			console.log(index, idx, id);
			// 修改哪一条数据索引
			console.log(this.equipmentKey, this.equipmentIdx1, this.equipmentIdx2);

			this.apparatusSelectIdx = [index, idx];
			let newObj = this.detailInfo[this.equipmentKey][this.equipmentIdx1].list[this.equipmentIdx2];
			newObj.equipment = this.apparatus[index].list[idx].name;
			this.detailInfo[this.equipmentKey][this.equipmentIdx1].list.splice(this.equipmentIdx2, 1, newObj);
		},
		showModal(e) {
			this.modalName = e.currentTarget.dataset.target;
			this.equipmentKey = e.currentTarget.dataset.key;
			this.equipmentIdx1 = e.currentTarget.dataset.idx1;
			this.equipmentIdx2 = e.currentTarget.dataset.idx2;
			this.kgKey = e.currentTarget.dataset.key;
			this.kgIdx1 = e.currentTarget.dataset.kgidx1;
			this.kgIdx2 = e.currentTarget.dataset.kgidx2;

			this.restKey = e.currentTarget.dataset.key;
			this.restIdx1 = e.currentTarget.dataset.restidx1;
			this.restIdx2 = e.currentTarget.dataset.restidx2;
			
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
		bindPickerChange(e, type, key, idx1, idx2) {
			console.log(e);
			switch (type) {
				case 1:
					let newObj = this.detailInfo[key][idx1].list[idx2];
					newObj.num = this.numList[0][e.detail.value[0]];
					newObj.numType = e.detail.value[1];
					// newObj.numType = this.numList[1][e.detail.value[1]];
					this.detailInfo[key][idx1].list.splice(idx2, 1, newObj);
					break;
			}
		},
		// 删除一项
		delOneParamete(index, idx, key) {
			if (this.detailInfo[key][index].list.length <= 1) {
				return;
			}
			this.detailInfo[key][index].list.splice(idx, 1);
		},
		gotoActionLibrary() {
			uni.navigateTo({
				url: '/pages/actionLibrary/actionLibrary?type=select'
			});
		}
	}
};
</script>

<style lang="scss">
.addTrainingClassView {
	padding-bottom: 65px;
	.topBox{
		font-size: 34rpx;
		padding-right: 25rpx;
		background-color: #4c515f;
		line-height: 30px;
		color: #fff;
		text-align: right;
	}
	& > .title {
		height: 50px;
		padding: 0 30rpx;
		margin-bottom: 24rpx;
		input {
			font-size: 28rpx;
		}
	}
	.section {
		margin-bottom: 18px;
		.titBox {
			padding: 30rpx;
			height: 50px;
			border-bottom: 1rpx solid #ededed;
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
					// height: 44px;
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
