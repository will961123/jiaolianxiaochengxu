<template>
	<!-- 添加动作图片/视频 -->
	<view class="addActionImgView">
		<view class="titBox">动作视频</view>
		<view class="imgBox videoBox bg-white">
			<view class="commitbox  ">
				<view class="cu-form-group" style="padding: 0;">
					<view class="grid col-4 grid-square flex-sub">
						<view class="solids" @tap="ChooseVideo"><text class="cuIcon-cameraadd"></text></view>
						<view class="bg-img" v-for="(item, index) in videoList" :key="index" :data-url="videoList[index]">
							<video :src="videoList[index]"></video> 
							<view class="cu-tag bg-red" @click.stop="DelVideo" :data-index="index"><text class="cuIcon-close"></text></view>
						</view>
					</view>
				</view>
			</view>
		</view>
		<view class="titBox">动作图片</view>
		<view class="imgBox videoBox bg-white">
			<view class="commitbox  ">
				<view class="cu-form-group" style="padding: 0;">
					<view class="grid col-4 grid-square flex-sub">
						<view class="solids" @tap="ChooseImage"><text class="cuIcon-cameraadd"></text></view>
						<view class="bg-img" v-for="(item, index) in imgList" :key="index" @tap="ViewImage" :data-url="imgList[index]">
							<image :src="imgList[index]" mode="aspectFill"></image>
							<view class="cu-tag bg-red" @click.stop="DelImg" :data-index="index"><text class="cuIcon-close"></text></view>
						</view>
					</view>
				</view>
			</view>
		</view>
		
		<button @click="back" class="btn cu-btn bg-blue">完成</button>
	</view>
</template>

<script>
export default {
	data() {
		return {
			imgList: [],
			videoList: []
		};
	},
	onUnload() {
		uni.setStorageSync('imgLength',this.imgList.length)
		uni.setStorageSync('videoLength',this.videoList.length)
	}, 
	methods: {
		back(){
			uni.navigateBack({
				delta:1
			})
		},
		ChooseVideo() {
			uni.chooseVideo({
				count: 1, //默认9
				sizeType: ['original', 'compressed'], //可以指定是原图还是压缩图，默认二者都有
				sourceType: ['album'], //从相册选择
				success: res => {
					if (this.videoList.length != 0) {
						this.videoList = this.videoList.concat(res.tempFilePath);
					} else {
						this.videoList = [res.tempFilePath];
					}
				}
			});
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
		ViewImage(e) {
			uni.previewImage({
				urls: this.imgList,
				current: e.currentTarget.dataset.url
			});
		},
		DelImg(e) {
			this.imgList.splice(e.currentTarget.dataset.index, 1);
		},
		DelVideo(e) {
			console.log(23333);
			this.videoList.splice(e.currentTarget.dataset.index, 1);
		}
	}
};
</script>

<style lang="scss">
.addActionImgView {
	.titBox {
		line-height: 50px;
		font-size: 32rpx;
		background-color: #f6f6f6;
		color: #666;
		padding: 0 30rpx;
	}
	.imgBox {
		padding: 35rpx 30rpx;
	}
	.videoBox {
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
