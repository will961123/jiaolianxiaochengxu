<template>
	<!-- 添加动作 -->
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
				<view @tap="showModal" data-target="ChooseModal" class="right text-right " style="width: 50%;">
					<view v-if="apparatusSelectList.length > 0">
						<text class="margin-right-xs" v-for="(item, index) in apparatusSelectList" :key="index">{{ item.name }}</text>
					</view>
					<view v-else>
						<text class=" text-gray">请选择</text>
						<text class="cuIcon  cuIcon-right text-gray"></text>
					</view>
				</view>
			</view>
			<view class="item flex align-center justify-between">
				<view class="name">阻力</view>
				<view @tap="showModal" data-target="Resistance" class="right text-right  flex align-center" style="width: 40%;">
					<!-- <text class=" text-gray">请选择</text> -->
					<input type="text" :disabled="true" :value="resistance" placeholder="请选择" />
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

			<view @tap="showModal" data-target="LabelStr" class="item flex align-center justify-between">
				<view class="name">标签</view>
				<view class="right text-right " style="width: 50%;">
					<view v-if="labelSelectList.length > 0">
						<text class="margin-right-xs" v-for="(item, index) in labelSelectList" :key="index">{{ item.name }}</text>
					</view>
					<view v-else>
						<text class=" text-gray">请选择</text>
						<text class="cuIcon  cuIcon-right text-gray"></text>
					</view>
				</view>
			</view>
			<view class="item flex align-center justify-between">
				<view class="name">共享设置</view>
				<view class="right text-right " style="width: 40%;">
					<picker @change="bindPickerChange($event, 6)" :range="shareTypeList">
						<text v-if="formData.shareType < 0" class=" text-gray">请选择</text>
						<text v-else style="line-height:60px ;" class="text-black">{{ shareTypeList[formData.shareType] }}</text>
						<text class="cuIcon  cuIcon-right text-gray"></text>
					</picker>
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
				<view @click="navgater('/pages/actionLibrary/addActionImg')" class="right text-right text-black " style="width: 40%;"> 
					<text v-if="imgLength > 0">{{ imgLength }}张图片</text>
					<text v-if="videoLength > 0 && imgLength > 0">,</text>
					<text v-if="videoLength > 0">{{ videoLength }}个视频</text>
					<text v-if="videoLength <= 0 && imgLength <= 0" class=" text-gray">请选择</text>
					<text v-if="videoLength <= 0 && imgLength <= 0" class="cuIcon  cuIcon-right text-gray"></text>
				</view>
			</view>
			<view class="item flex align-center justify-between">
				<view class="name">动作要领</view>
				<view @click="navgater('/pages/actionLibrary/addActionPoints')" class="right text-right " style="width: 40%;">
					<text class=" text-gray">请选择</text>
					<text class="cuIcon  cuIcon-right text-gray"></text>
				</view>
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
								:class="item.select ? 'bg-blue' : ''"
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
						<view @click.stop="changeResistanceType(index)" v-for="(item, index) in resistanceTypeList" :key="index" :class="index === resistanceType ? 'item select' : 'item'">
							{{ item }}
						</view>
					</view>
					<view class="ipt bg-white flex align-center justify-between">
						<input :focus="focus" v-model="resistance" type="digit" placeholder="请输入" />
						<view @tap.stop="hideModal" class="text-blue">确定</view>
					</view>
				</view>
			</view>
		</view>

		<!-- 标签 -->
		<view class="cu-modal bottom-modal " :class="modalName == 'LabelStr' ? 'show' : ''" @tap="hideModal">
			<view style="padding: 0;" class="cu-dialog bg-white" @tap.stop="">
				<view class="cu-bar flex justify-between">
					<view class="action text-blue" @tap="hideModal">取消</view>
					<view class="tit text-bold">设置标签</view>
					<view class="action text-green " @tap="hideModal">确定</view>
				</view>
				<view v-for="(item, index) in labelList" :key="index" class="labelItem flex justify-between">
					<text class="name">{{ item.name }}</text>
					<label @click="item.select = !item.select" class="radio blue">
						<radio :checked="item.select" value="" />
						<text></text>
					</label>
				</view>
				<view class="iptbox2">
					<view class="ipt bg-white flex align-center justify-between">
						<input v-model="labelStr" type="text" placeholder="+新建标签" />
						<view @tap.stop="addLabelObj" class="text-blue">确定</view>
					</view>
				</view>
			</view>
		</view>

		<button @click="save" class="btn cu-btn bg-blue">完成</button>
	</view>
</template>

<script>
export default {
	data() {
		return {
			focus: false,
			focus2: false,
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
				position: -1,
				shareType: 0 // 共享类型
			},

			modalName: '',

			// 这几个可选器械数据模型
			apparatus: [],
			apparatusSelectList: [],
			showAddIpt: false,
			addApparatuStr: '',

			// 阻力数据模型
			resistance: '',
			resistanceType: 0,
			resistanceTypeList: ['KG', 'LBS', '级'],

			// 标签数据模型
			labelStr: '',
			labelList: [],

			// 共享类型数据模型
			shareTypeList: ['不共享', '团队共享', '连锁店共享'],

			imgLength: 0,
			videoLength: 0
		};
	},
	computed: {
		labelSelectList() {
			return this.labelList.filter(v => {
				return v.select === true;
			});
		}
	},
	onShow() {
		this.imgLength = uni.getStorageSync('imgLength') || 0;
		this.videoLength = uni.getStorageSync('videoLength') || 0;
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
		this.labelList = [{ name: '热身', id: 1, select: false }, { name: '正式', id: 2, select: false }, { name: '放松', id: 3, select: false }];
	},

	methods: {
		changeResistanceType(type){
			this.resistanceType = type 
			this.focus = false; 
			setTimeout(()=>{  
				this.focus = true;
			},500) 
			
		},
		save(){
			uni.navigateBack({
				delta:1
			})
		},
		navgater(path) {
			uni.navigateTo({
				url: path
			});
		},
		addLabelObj() {
			if (this.labelStr) {
				let newLabelObj = {
					name: this.labelStr,
					id: new Date().getTime(),
					select: false
				};
				this.labelStr = '';
				this.labelList.push(newLabelObj);
			}
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
		selectOne(first, idx, type) {
			if (type === -1) {
				this.showAddIpt = true;
				return false;
			}

			let saveListIdx;
			let newSelectObj = this.apparatus[first].list[idx];
			for (let i in this.apparatusSelectList) {
				if (this.apparatusSelectList[i].id === newSelectObj.id) {
					saveListIdx = i;
				}
			}
			if (
				this.apparatusSelectList.some((v, i) => {
					return v.id === newSelectObj.id;
				})
			) {
				newSelectObj.select = false;
				this.apparatus[first].list.splice(idx, 1, newSelectObj);
				this.apparatusSelectList.splice(saveListIdx, 1);
			} else if (this.apparatusSelectList.length >= 6) {
				uni.showToast({
					title: '最多选择6个',
					icon: 'none'
				});
			} else {
				newSelectObj.select = true;
				this.apparatus[first].list.splice(idx, 1, newSelectObj);
				this.apparatusSelectList.push(newSelectObj);
			}
		},
		showModal(e) {
			this.modalName = e.currentTarget.dataset.target;
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
		ChooseCheckbox(e) {
			let items = this.checkbox;
			let values = e.currentTarget.dataset.value;
			for (let i = 0, lenI = items.length; i < lenI; ++i) {
				if (items[i].value == values) {
					items[i].checked = !items[i].checked;
					break;
				}
			}
		},
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
				case 6:
					this.formData.shareType = e.detail.value;
					break;
			}
		}
	}
};
</script>

<style lang="scss">
.addActionView {
	padding-bottom: 64px;
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
					height: 44px;
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

	.btn {
		border-radius: 0;
		width: 100%;
		height: 60px;
		position: fixed;
		left: 0;
		bottom: 0;
	}
}
</style>
