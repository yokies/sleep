<template>
    <view class='room-container' 
        @touchend='tend'>
        <image class="room-image" src="../../static/room/inroom-background.png">
            
        </image>
        <view class='person-box'>
            <view v-if='showchat' class="chat-box">
                <view class='text-box'>
                    <text>你在这里</text>
                </view>
                <view class="triangle"></view>
            </view>
            <view class='chara-box'>
                <image class="character" src='../../static/testchara.png'></image>
            </view>
        </view>
    </view>
</template>

<script>
    export default {
        data (){
            return {
                showchat:true,
                positionX:'',
                positionY:'',
            }
        },
        methods:{
            tend(e){
                console.log(e)
            }
        },
        onShow() {
            setTimeout(()=>{
                this.showchat = false
            }, 2000)
        }
    }
</script>

<style>
    .room-image{
        position: fixed;
        width: 100%;
        height: 100%;
        top: 0;
        left: 0;
        z-index: 0;
    },
    .person-box{
        width: 100px;
        position: absolute;
        left: 181px;
        top: 300px;
    },
    .chat-box{
        width: 100%;
    },
    .text-box{
        text-align: center;
        background-color: #F7F7F7;
        border-radius: 6rpx;
        padding:20rpx;
    },
    .triangle {
        border-left: 15px solid transparent;
        border-right: 10px solid transparent;
        border-top: 10px solid #F7F7F7;
        border-radius: 6rpx;
        width: 1px;
        margin-left: 15px;
    },
    .chara-box{
        position: absolute;
        top: 40px;
        left: 12px;
    },
    .character{
        width: 74px;
        height: 112px;
    }
</style>
