<template>
	<view>
		<view class="self">
			<image src="../../static/images/user.jpg" mode="aspectFill"></image>
			<view class="info">
				<text>姓名：</text>
				<text>电话：</text>
				<text>邮箱：</text>
				<text>地址：</text>
			</view>
		</view>
		
		<view class="history">
			<image src="../../static/icons/history.png" mode="aspectFill"></image>
			<view class="text">
				<text>历史记录</text>			
			</view>
		</view>
		
		<view class="content">
			<view class="row" v-for="item in historyList">
				<newsItem :item="item"></newsItem>
			</view>
		</view>
		
	</view>
</template>

<script>
	export default {
		onLoad() {
			this.getData()
		},
		onShow() {
			this.getData()
		},
		data() {
			return {
				historyList:[]
			};
		},
		methods:{
			getData(){
				this.historyList=uni.getStorageSync("historyList")
			}
		}
	}
</script>

<style lang="scss">
	.self{
		padding: 20rpx;
		display: flex;
		border-bottom:1px dashed #ddd;
		image{
			width: 200rpx;
			height: 200rpx;
		}
		.info{
			//border: 1px solid #ddd;
			padding-left: 20rpx;
			display: flex;
			flex-direction: column;
			justify-content: space-between;
		}
	}
	
	.history{
		padding: 20rpx;
		display: flex;
		image{
			width: 50rpx;
			height: 50rpx;
		}
		.text{
			padding: 3rpx 15rpx;
			font-size: 30rpx;
		}
	}
	
	.content{
		padding: 20rpx;
		.row{
			border-bottom:1px dashed #F7F7F7;
			padding: 20rpx 0;
		}
	}

</style>
