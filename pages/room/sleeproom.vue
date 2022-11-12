<template>
    <view class='room-container' 
        @touchstart='tstart' @touchend='tend' @touchmove='tmove'>
        <image class="room-image" src="../../static/room/inroom-background.png">
            
        </image>
    </view>
</template>

<script>
    export default {
        data (){
            return {
                positionX:'',
                positionY:'',
            }
        },
        methods:{
            tstart(e){
                console.log(e)
            }
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
        z-index: -1;
    }
</style>
