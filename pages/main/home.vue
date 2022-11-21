<template>
        <view class='home-container'>
            <view class='imgbox'>
                <image class='home-img' src="../../static/home/home-background.png">
                </image>
            </view>
            <view class='txtbox'>
                <view class='timebox'>
                    <view style = 'font-size: 26px; color: #9C9C9C;'>
                        {{dateFormat(date).day}}
                    </view>
                    <view style = 'font-size: 26px; color: #838ABD; margin-left: 30px;'>
                        {{dateFormat(date).time}}
                    </view>
                </view>
                <view class='title'>
                    <text>Sleep House</text>
                </view>
                <view>
                    <text clss='text'>欢迎来到北航早睡社区，我们希望每一位北航同学都能够获得高质量睡眠，在这里您可以获得有趣的睡眠体验。</text>
                </view>
            </view>
        </view>
</template>

<script>
    export default {
        data() {
            return {
                date: new Date().toISOString(),
            }
        },
        methods:{
            dateFormat(d) {
                let date = new Date(d)
                let year = date.getFullYear()
                let month = date.getMonth() + 1 < 10 ? "0" + (date.getMonth() + 1) : date.getMonth() + 1
                let day = date.getDate() < 10 ? "0" + date.getDate() : date.getDate()
                let hours = date.getHours() < 10 ? "0" + date.getHours() : date.getHours()
                let minutes = date.getMinutes() < 10 ? "0" + date.getMinutes() : date.getMinutes()
                let seconds = date.getSeconds() < 10 ? "0" + date.getSeconds() : date.getSeconds()
                
                return {
                    day : year + "年" + month + "月" + day + "日",
                    time : hours + ":" + minutes + ":" + seconds
                }
            }
        },
        onLoad() {
            let that = this
            setInterval(()=>{
                that.date = Date.parse(new Date)
            }, 1000)
        }
    }
</script>

<style>
    .home-container{
        display: flex;
        justify-content: space-between;
        flex-direction: column;
        height: 100%;
        width: 100%;
    },
    .timebox{
        display: flex;
        align-items: center;
    }
    .txtbox{
        flex: 3;
        margin: 5px 30px;
    },
    .imgbox{
        display: flex;
        flex: 7;
    },
    .home-img{
        height: 100%;
        width: 100%;
    }
    .title{
        margin-top: 8px;
        margin-bottom: 10px;
        font-size: 24px;
        font-weight: 500;
    },
    .text{
        font-size: normal;
        line-height: 24px;
        font-size: 16px;
        font-weight: 400;
    }
</style>
