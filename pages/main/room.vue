<template>
    <view class='room-container'>
        <view class='tip' v-if='showTip'>
            <view class='tip-text'>
                <view class='title'>
                    什么是Sleep House？
                </view>
                <view class='text'>
                    “Sleep House”是一个将您的睡眠状态进行实时可视化的圈子。在这里您可以在虚拟的Sleep House中获得有趣的体验～
                </view>
            </view>
            <view class='triangle'>
            </view>
        </view>
        
        <image class="room-image" src="../../static/room/room-background.png">
            
        </image>
        <button class="tip-button" @click="show_tip">
            <view style="color: #FFFFFF;">什么是Sleep House？</view>
        </button>   
        <button class="room-button" @click="visit_room">
            <view style="color: #FFFFFF;">进入Sleep House</view>
        </button>   
    </view>
</template>

<script>
    export default {
        data(){
            return {
                showTip: false
            }
        },
        methods: {
            visit_room() {
                uni.navigateTo({
                    url: "../room/sleeproom"
                })
            },
            show_tip() {
                this.showTip = !this.showTip
            }
        }
    }
</script>

<style>
    .room-container{
        height: 100%;
        width: 100%;
    },
    .room-image{
        position: fixed;
        width: 100%;
        height: 100%;
        top: 0;
        left: 0;
        z-index: -1;
    },
    .tip-button{
        position: absolute;
        bottom: 20%;
        left: 15%;
        width: 70%;
        background-color: #899BF1;
    },
    .room-button{
        position: absolute;
        bottom: 10%;
        left: 15%;
        width: 70%;
        background-color: #899BF1;
    },
    .tip{
        background-color: #899BF1;
        border-radius: 20px;
        position: absolute;
        bottom: 35%;
        left: 10%;
        width: 80%;
        height: 50%;
    },
    .tip-text{
        height: 80%;

    },
    .triangle {
        border-left: 50px solid transparent;
        border-right: 50px solid transparent;
        border-top: 50px solid #899BF1;
        border-radius: 6rpx;
        width: 1px;
        margin-left: 35%;
    },
    .title{
        text-align: center;
        color: #FFFFFF;
        font-size: 22px;
        font-weight: 500;
        margin-top: 15%;
    },
    .text{
        text-align: left;
        color: #FFFFFF;
        font-size: 18px;
        font-weight: 400;
        margin-top: 20px;
        margin-left: 25px;
        margin-right: 25px;
    }
</style>
