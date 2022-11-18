<template>
    <view class="login-back">
        <view class='logo-container'>
            <image class='logo' src='../../static/login/LOGO.png'></image>
            <text class="title">Sleep House</text>
        </view>
    </view>
</template>

<script>
    export default{
        onShow(){
            setTimeout(()=>{
                uni.switchTab({
                    url:'../main/home'
                })
            }, 2000)
        }
    }
</script>

<style>
    .login-back{
        width: 100%;
        height: 100%;
        background-color: #C9CDFF;
        display: flex;
    },
    .logo-container{
        margin:auto auto;
        display: flex;
        flex-direction: column;
        align-items: center;
    },
    .logo{
        width: 124px;
        height: 124px;
    },
    .title{
        font-size: large;
        font-weight: 600;
    }
</style>
