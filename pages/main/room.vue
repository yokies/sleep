<template>
    <view class='room-container'>
        <image class="room-image" src="../../static/room/room-background.png">
            
        </image>
        <button type="primary" class="room-button" @click="visit_room">
            进入房间
        </button>   
    </view>
</template>

<script>
    export default {
        methods: {
            visit_room() {
                uni.navigateTo({
                    url: "../room/sleeproom"
                })
            }
        }
    }
</script>

<style>
    .room-container{
        
    },
    .room-image{
        position: fixed;
        width: 100%;
        height: 100%;
        top: 0;
        left: 0;
        z-index: -1;
    },
    .room-button{
        position: absolute;
        bottom: 10%;
        left: 15%;
        width: 70%;
    }
</style>
