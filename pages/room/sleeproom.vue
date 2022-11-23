<template>
    <view class='room-container' 
        @touchend='tend'>
        <image class="room-image" src="../../static/room/inroom-background.png">
            
        </image>
        <view v-if='!sleeping' class='person-box'>
            <view v-if='chatbox' class="chat-box">
                <view class='text-box'>
                    <text>你在这里</text>
                </view>
                <view class="triangle"></view>
            </view>
            <view class='chara-box'>
                <image class="character" :src='personImage'>
                    
                </image>
            </view>
        </view>
        
        <view v-if='sleeping' class="bed-button-1">
            <view style='font-size: 20px; font-weight: 500;'>
                {{timeFormat(sleepTime)}}
            </view>
        </view>
        <view v-if='sleeping' class="bed-button" @click="wakeup()">
            <view style='font-size: 20px; font-weight: 500;'>
                起床
            </view>
        </view>        
        <view v-if='!bedbox && !sleeping' class='bed-button' @click="showbed()">
            <view style='font-size: 20px; font-weight: 500;'>
                开始睡觉
            </view>
        </view>
        
        <!-- 选择床位-->
        <view v-if='bedbox' class='bed-box'>
            <view class='bed-title'>
                请选择你的床位
            </view>
            <view class='bed-content'>
                <view v-for='bed in bedlist' :key='bed.id' class='bed'
                @click="choseBed(bed)">
                    <view class='id-container' 
                    :class='bed.id==chosedBedId?"bed-chose":
                    (bed.canchoose?"bed-not-chosed":"bed-chosed")'>
                        {{bed.id}}号床
                    </view>
                </view>
            </view>
            <view class="ok-button" @click="sleep()">
                OK
            </view>
        </view>
        
        <view v-for='bed in bedlist' :key='bed.id'
                v-if = '!bed.canchoose || bed.id == sleepBedId'
                style = 'position: absolute;'
                :style='{left:calleft(bed.id), top:caltop(bed.id)}'>
            <image :src='sleepImage' class='sleep-man'></image>
            <view v-if='showId==bed.id' class="sleep-chat-box">
                <view class='text-box'>
                    <text>你在这里</text>
                </view>
                <view class="triangle"></view>
            </view>
        </view>
    </view>
</template>

<script>
    export default {
        data (){
            return {
                chatbox: true,
                bedbox: false,
                positionX: '',
                positionY: '',
                personImage: '../../static/testchara.png',
                sleepImage: '../../static/room/sleep-man.png',
                chosedBedId: 0,
                sleepBedId: 0,
                sleeping: false,
                bedlist:[
                    {id: 1, canchoose: true},
                    {id: 2, canchoose: true},
                    {id: 3, canchoose: false},
                    {id: 4, canchoose: true},
                    {id: 5, canchoose: true},
                    {id: 6, canchoose: true},
                    {id: 7, canchoose: true},
                    {id: 8, canchoose: false}
                ],
                timer:'',
                sleepTime: 0,
                showId: 0,
            }
        },
        methods:{
            tend(e){
            },
            showbed(){
                this.bedbox = !this.bedbox
            },
            choseBed(bed){
                if (!bed.canchoose){
                    return
                }
                if (this.chosedBedId == bed.id) {
                    this.chosedBedId = 0
                } else {
                    this.chosedBedId = bed.id
                }   
            },
            sleep(){
                this.sleepBedId = this.chosedBedId
                this.bedbox = !this.bedbox
                this.sleeping = true
                this.timer = setInterval(()=>{
                    let that = this
                	this.sleepTime++
                }, 1000)
                this.showId = this.sleepBedId
                setTimeout(()=>{
                    this.showId = 0
                }, 2000)
            },
            wakeup(){
                this.chosedBedId = 0
                this.sleepBedId = 0
                this.sleeping = false
                this.sleepTime = 0
                clearInterval(this.timer)
            },
            timeFormat(time){
                let hours = Math.floor(time / 3600)
                if (hours < 10){
                    hours = '0' + hours
                }
                let minutes = Math.floor(time / 60)
                if (minutes < 10){
                    minutes = '0' + minutes
                }
                let seconds = time % 60
                if (seconds < 10){
                    seconds = '0' + seconds
                }
                return hours + ':' + minutes + ':' + seconds
            },
            calleft(id){
                return 14.5 + 29 * ((id - 1) % 2) + '%'
            },
            caltop(id){
                if (id % 2 == 0) {
                    return 3 + 9.2 * Math.floor((id - 1) / 2) + '%'
                } else{
                    return 9.5 + 9.4 * Math.floor((id - 1) / 2) + '%'
                }
                
            }
        },
        onShow() {
            setTimeout(()=>{
                this.chatbox = false
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
    },
    .person-box{
        width: 100px;
        position: absolute;
        left: 181px;
        top: 300px;
    },
    .sleep-chat-box{
        width: 130%;
        position: absolute;
        top: -30%;
        left: -5%;
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
    },
    .bed-button-1{
        z-index: 3;
        position: absolute;
        background-color: #FFFFFF;
        width: 40%;
        height: 8%;
        bottom: 20%;
        left: 30%;
        box-shadow: 1px 2px 10px rgba(0, 0, 0, 0.2);
        border-radius: 20px;
        display: flex;
        align-items: center;
        justify-content: center;
    },
    .bed-button{
        z-index: 3;
        position: absolute;
        background-color: #FFFFFF;
        width: 40%;
        height: 8%;
        bottom: 10%;
        left: 30%;
        box-shadow: 1px 2px 10px rgba(0, 0, 0, 0.2);
        border-radius: 20px;
        display: flex;
        align-items: center;
        justify-content: center;
    },
    .bed-box{
        z-index: 4;
        position: absolute;
        width: 80%;
        height: 50%;
        bottom: 30%;
        left: 10%;
        background-color: #FFFFFF;
        border-radius: 20px;
        display: flex;
        flex-direction: column;
        align-items: center;
    },
    .bed-title{
        font-weight: 500;
        font-size: 20px;
        display: flex;
        align-items: center;
        justify-content: center;
        flex:3;
    },
    .bed-content{
        flex:16;
        display:flex;
        flex-wrap: wrap;
    },
    .ok-button{
        flex:3;
        display: flex;
        align-items: center;
        justify-content: center;
        box-shadow: 1px 1px 10px rgba(0, 0, 0, 0.2);
        border-radius: 20px;
        width: 30%;
        margin-bottom: 10px;
    },
    .bed{
        width: 49%;
        display: flex;
        align-items: center;
        justify-content: center;
    },
    .id-container{
        width: 112px;
        height: 38px;
        border-radius: 20px;
        display: flex;
        align-items: center;
        justify-content: center;
    },
    .bed-chose{
        color: #FFFFFF;
        background-color: #A8695A;
    },
    .bed-not-chosed{
        background-color: #FFBDAD;
    }
    .bed-chosed{
        color: #C7C7C7;
        background-color: #FFD9D0;
    },
    .sleep-man{
        width: 66px;
        height: 69px;
    }
</style>
