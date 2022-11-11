<template>
        <view class='home-container'>
            <view class='imgbox'>
                <image class='home-img' src="../../static/home/6.png">
                </image>
            </view>
            <view class='txtbox'>
                <view>
                    <h2 style="margin-bottom: 20px; color: ;">北航早睡</h2>
                </view>
                <view>
                    <text style="font-size: large">欢迎来到北航早睡app<br>这里还要加点什么呢</text>
                </view>
            </view>
        </view>
</template>

<script>
</script>

<style>
    .home-container{
        display: flex;
        justify-content: space-between;
        flex-direction: column;
        height: 100%;
        width: 100%;
    },
    .txtbox{
        flex: 2;
        margin: 5px 30px;
    },
    .imgbox{
        display: flex;
        flex: 8;
        justify-content: center;
        align-items: center;
    },
    .home-img{
        height: 100%;
        width: 100%;
    }
</style>
