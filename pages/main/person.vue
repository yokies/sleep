<template>
	<view>
		<!-- #ifdef APP-PLUS -->
		<!-- #endif -->
		<image class="person-background" src="../../static/home/person-background.png">
			
		</image>
		<view class="photo-collection" v-if="this.step==0">
			<text class="title">
				拍摄面部
			</text>
			<view class="pic-box"></view>
			<view class="button" @click="take_photo">
				<image class="icon" src="../../static/home/takephoto.png"></image>
			</view>
		</view>
		<view class="photo-transfer" v-if="this.step==1">
			<view class="pic-box">
				<!--
				<image class="picture" :src="this.imgUrl"></image>
				-->
				<image class="picture" src="../../static/testchara.png"></image>
			</view>
			<text class="msg-text">
				已生成您的虚拟形象
			</text>
			<view class="adopt-button" @click="photo_adopt">
				下一步
			</view>
			<view class="cancel-button" @click="photo_retake">
				取消
			</view>
		</view>
		
	</view>
</template>

<script>
	// import html2canvas from 'html2canvas'
	import {pathToBase64} from 'image-tools'
	export default {
		data() {
		  return {
			step: 0,
			imgPathList: [],
			imgUrl:"",
			blobFile: null,
			canvas: null,
			video:null,
			mediaStreamTrack: '',
			getFaceFeatureUrl: "https://aip.baidubce.com/rest/2.0/face/v3/detect",
			getAccessTokenUrl: "https://aip.baidubce.com/oauth/2.0/token",
			// access_token date: 2022/11/16
			access_token: "24.c69b2eed96a6bc259f6f0c9f3eb00c83.2592000.1671190969.282335-28465145",
			imgBase64: ''
		  };
		},
		/*
		onShow() {
		  // 摄像头
		  this.video = document.querySelector('video');
		},
		*/
		methods: {
			//点击拍照截图画面
			take_photo() {
			    uni.chooseImage({
			    	count: 1,
			    	sourceType: ['camera', 'album'],
			    	success: res => {
						this.imgPathList.push(res.tempFilePaths[0]);
						this.imgUrl = res.tempFilePaths[0];
						this.step = 1;
			        	console.log(res.tempFilePaths[0]);
						pathToBase64(this.imgUrl).then(path => {
							this.imgBase64 = path.split(',')[1];
							// console.log(this.imgBase64);
							uni.request({
								header: {
									"content-type": "application/json"
								},
								url: this.getFaceFeatureUrl + "?access_token=" + this.access_token,
								method: 'POST',
								data: {
									'image': this.imgBase64,
									'image_type': 'BASE64'
								},
								dataType: 'json',
								success: (res) => {
									console.log('face_feature_json', res.data);
									/*
									uni.showToast({
										title: 'OK!!!',
										icon:'none',
										duration: 2000
									});
									*/
								},
								fail: (error) => {
									console.log('error', error.errMsg);
									uni.showToast({
										title: 'NO!!!',
										icon:'none',
										duration: 2000
									});
								}
							});
						});
						
			    	},
			    	fail: (err) => {
			    		console.log('error', err);
			    	}
			    });			    
			},
			photo_retake() {
				this.step = 0;
			},
			photo_adopt() {
				uni.navigateTo({
				    url: "../room/sleeproom"
				});
			}
		}
	}
	
</script>

<style lang="scss">
.person-background {
	position: absolute;
	width: 100%;
	height: 100%;
}
	
.photo-collection {
	.pic-box {
		position: absolute;
		width: 600rpx;
		height: 700rpx;
		left: 75rpx;
		top: 250rpx;
		background-color: #D9D9D9;
	}
	
	.title {
		position: absolute;
		text-align: center;
		font-weight: bold;
		font-size: 60rpx;
		top: 7%;
		width: 500rpx;
		left: 125rpx;
	}

	.button {
		position: absolute;
		width: 120rpx;
		height: 120rpx;
		left: 315rpx;
		bottom: 200rpx;
			
		background: #FFFFFF;
		box-shadow: 0rpx 0rpx 20rpx rgba(0, 0, 0, 0.3);
		border-radius: 15px;
		.icon {
			left: 20rpx;
			top: 20rpx;
			width: 80rpx;
			height: 80rpx;
		}
	}
}

.photo-transfer {
	.msg-text {
		position: absolute;
		text-align: center;
		font-weight: bold;
		font-size: 45rpx;
		top: 72%;
		width: 550rpx;
		left: 100rpx;
	}

	.pic-box {
		position: absolute;
		width: 600rpx;
		height: 800rpx;
		left: 75rpx;
		top: 130rpx;
		background-color: white;
		.picture {
			width: 600rpx;
			height: 800rpx;
		}
	}

	.adopt-button {
		position: absolute;
		width: 200rpx;
		height: 100rpx;
		left: 150rpx;
		bottom: 175rpx;
		text-align: center;
		line-height: 100rpx;
		font-size: 40rpx;
			
		background: #FFFFFF;
		box-shadow: 0rpx 0rpx 20rpx rgba(0, 0, 0, 0.3);
		border-radius: 15px;
	}

	.cancel-button {
		position: absolute;
		width: 200rpx;
		height: 100rpx;
		left: 400rpx;
		bottom: 175rpx;
		text-align: center;
		line-height: 100rpx;
		font-size: 40rpx;
			
		background: #FFFFFF;
		box-shadow: 0rpx 0rpx 20rpx rgba(0, 0, 0, 0.3);
		border-radius: 15px;
	}
	
}

</style>
