<template>
	<view>
		<!-- #ifdef APP-PLUS -->
		<!-- #endif -->
		<image class="person-background" src="../../static/home/person-background.png"></image>
		
		<!-- step 0: 拍摄面部 -->
		<view class="photo-collection" v-if="this.step==0">
			<text class="title">
				拍摄面部
			</text>
			<view class="pic-box"></view>
			<view class="button" @click="take_photo()">
				<image class="icon" src="../../static/home/takephoto.png"></image>
			</view>
		</view>
		
		<!-- step 1: 生成头像 -->
		<view class="photo-transfer" v-if="this.step==1">
			<view class="pic-box">
				<!--
				<image class="picture" :src="this.imgUrl"></image>
				-->
				<image class="picture" src="../../static/testchara.png"></image>
			</view>
			<text class="msg-text">
				已生成您的头像
			</text>
			<view class="next-button" @click="next_step()">
				下一步
			</view>
			<view class="cancel-button" @click="photo_retake()">
				取消
			</view>
		</view>
		
		<!-- step 2: 组件选择 -->
		<view class="photo-transfer" v-else-if="this.step==2">
			
			<view class="back-view" @click="step_back()">					
				<image class="icon-back" src="../../static/home/person-back.png"></image>
			</view>
			<view class="next-view" @click="next_step()">
				<image class="icon-next" src="../../static/home/person-next.png"></image>
			</view>
			
			<view class="online-display">
				<image class="overall-display" src="../../static/testchara.png"></image>
			</view>
			
			<view class="trapezoid" style="left: 45rpx;">
				<text class="title-bold" v-if="this.componentColumn==0">
					服饰
				</text>
				<text class="title" v-else @click="change_component_column(0)">
					服饰
				</text>
			</view>
			<view class="trapezoid" style="left: 210rpx;">
				<text class="title-bold" v-if="this.componentColumn==1">
					配饰
				</text>
				<text class="title" v-else @click="change_component_column(1)">
					配饰
				</text>
			</view>
			<view class="trapezoid" style="left: 375rpx;">
				<text class="title-bold" v-if="this.componentColumn==2">
					特效
				</text>
				<text class="title" v-else @click="change_component_column(2)">
					特效
				</text>
			</view>
			<view class="trapezoid" style="left: 540rpx;">
				<text class="title-bold" v-if="this.componentColumn==3">
					道具
				</text>
				<text class="title" v-else @click="change_component_column(3)">
					道具
				</text>
			</view>
			<view class="component-columns">
				<view class="components" v-if="this.componentColumn==0">
					服饰列表……
				</view>
				<view class="components" v-if="this.componentColumn==1">
					配饰列表……
				</view>
				<view class="components" v-if="this.componentColumn==2">
					特效列表……
				</view>
				<view class="components" v-if="this.componentColumn==3">
					道具列表……
				</view>
			</view>
		</view>
			
			
			
			
			
			
			
		<!-- step 3: 生成虚拟形象 -->
		<view class="photo-transfer" v-if="this.step==3">
			<view class="pic-box">
				<!--
				<image class="picture" :src="this.imgUrl"></image>
				-->
				<image class="picture" src="../../static/testchara.png"></image>
			</view>
			<text class="msg-text">
				您的虚拟形象已形成！
			</text>
			<view class="adopt-button" @click="next_step()">
				OK
			</view>
		</view>
		<!-- step 4: 展示形象 -->
		<view class="photo-transfer" v-if="this.step==4">
			<view class="pic-box">
				<!--
				<image class="picture" :src="this.imgUrl"></image>
				-->
				<image class="picture" src="../../static/testchara.png"></image>
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
			imgBase64: '',
			componentColumn: 0
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
						this.next_step();
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
			next_step() {
				this.step = this.step + 1;
				console.log('step', this.step);
			},
			step_back() {
				this.step = this.step - 1;
				console.log('step', this.step);
			},
			photo_adopt() {
				uni.navigateTo({
				    url: "../room/sleeproom"
				});
			},
			change_component_column(column_num) {
				this.componentColumn = column_num;
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
		color: #FFFFFF;
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
		font-size: 50rpx;
		top: 8%;
		width: 550rpx;
		left: 100rpx;
		color: #FFFFFF;
	}

	.pic-box {
		position: absolute;
		width: 600rpx;
		height: 800rpx;
		left: 75rpx;
		top: 230rpx;
		background: white;
		border-radius: 30rpx;
		.picture {
			width: 600rpx;
			height: 850rpx;
		}
	}
	
	.next-button {
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

	.adopt-button {
		position: absolute;
		width: 200rpx;
		height: 100rpx;
		left: 275rpx;
		bottom: 175rpx;
		text-align: center;
		line-height: 100rpx;
		font-size: 40rpx;
			
		background: #FFFFFF;
		box-shadow: 0rpx 0rpx 20rpx rgba(0, 0, 0, 0.3);
		border-radius: 15px;
	}
	
	
	.next-view{
		position: absolute;
		right: 0rpx;
		width: 120rpx;
		height: 120rpx;
		background: #D9D9D9;	
		
		.icon-next {
			position: absolute;
			right: 30rpx;
			top: 50rpx;
			width: 60rpx;
			height: 60rpx;
			.full-view {
				height: 100%;
				width: 100%;
			}
		}
	}
	
	.back-view {
		position: absolute;
		left: 0rpx;
		width: 120rpx;
		height: 120rpx;
		background: #D9D9D9;
		
		.icon-back {
			position: absolute;
			left: 30rpx;
			top: 50rpx;
			width: 60rpx;
			height: 60rpx;
			background: #D9D9D9;
			.full-view {
				height: 100%;
				width: 100%;
			}
		}
	}
	
	
	.online-display {
		position: absolute;
		top: 30rpx;
		width: 100%;
		height: 45%;
		// background: #D9D9D9;
		.overall-display {
			display: block;
			margin: 0 auto;
			padding-right: 30rpx;
			width: 450rpx;
			height: 700rpx;
		}
	}
	
	.trapezoid {
		position: absolute;
		bottom: 45%;
		left: 45rpx;
		width: 105rpx;
		height: 0rpx;
		border-bottom: 60rpx solid;
		border-left: 30rpx solid;
		border-right: 30rpx solid;
		border-color: transparent transparent #F0F0F0 transparent;
		.title {
			line-height: 60rpx;
			font-size: 36rpx;
			padding-left: 15rpx;
		}
		.title-bold {
			line-height: 60rpx;
			font-size: 36rpx;
			padding-left: 15rpx;
			font-weight: bold;
		}
	}
	
	.component-columns {
		position: absolute;
		top: 55%;
		width: 100%;
		height: 1000rpx;
		border-radius: 30rpx;
		background: #F8F8F8;
		.components {
			width: 100%;
			
			// temporary -> wait for revision
			height: 500rpx;
			text-align: center;
			font-size: 40rpx;
			line-height: 300rpx;
			
		}
	}
}

</style>
