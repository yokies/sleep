<template>
    <view class="uni-container">
        <a>登录页面</a>
        <button class="" style="margin-top: 10rpx;" @click="bindVisit()">
            只是个登录跳转
        </button>
    </view>
</template>

<script>
    export default{
        methods: {
            bindVisit() {
                uni.switchTab({
                    url: '../main/home'
                })
            }
        }
    }
    
</script>

<style>
</style>
