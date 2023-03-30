<template>
	<view class="detail">
		
		<view class="title">{{detail.title}}</view>
		
		<view class="info">
			<view class="author">编辑：{{detail.author}}</view>
			<view class="date">发布时间：{{detail.posttime}}</view>
		</view>
		
		<view class="content">
			<rich-text :nodes="detail.content"></rich-text>
		</view>
		
		<view class="copyRight">
			声明：本站的内容均采集与腾讯新闻，如果侵权请联系管理（1572307127@qq.com）
		进行整改删除，本站进行了内容采集不代表本站及作者观点，若有侵犯请及时联系管理员，谢谢您的支持。
		</view>
		
	</view>
</template>

<script>
	import {parseTime} from "../../utils/tool.js"
	export default {
		onLoad(e) {
			this.vars=e,
			this.getDetail()
		},
		data() {
			return {
				vars:{},
				detail:{}
			};
		},
		methods:{
			getDetail(){
				uni.showLoading({
					title:"加载中..."
				})
				uni.request({
					url:"https://ku.qingnian8.com/dataApi/news/detail.php",
					data:this.vars,
					success: (res) => {
						res.data.posttime=parseTime(res.data.posttime),
						res.data.content=res.data.content.replace(/<img/gi,'<img style="max-width:100%"')
						this.detail=res.data,
						uni.hideLoading(),
						this.saveHistory()
					}
				})
			},
			saveHistory(){
				let historyList=uni.getStorageSync("historyList")||[]
				let item={
					id:this.detail.id,
					classid:this.detail.classid,
					picurl:this.detail.picurl,
					title:this.detail.title,
					looktime:parseTime(Date.now())
				}
				let index=historyList.findIndex(i=>{
					return i.id==this.detail.id
				})
				
				if(index>=0){
					historyList.splice(index,1)
				}
				historyList.unshift(item)
				historyList=historyList.slice(0,10)
				uni.setStorageSync("historyList",historyList)
			}
		}
	}
</script>

<style lang="scss">
	.detail{
		padding: 20rpx;
		.title{
			font-size: 50rpx;
			padding: 15rpx;
			display: flex;
			align-content: center;
		}
		.info{
			display: flex;
			justify-content: space-between;
			padding: 25rpx;
			background-color: #f6f6f6;
			font-size: 24rpx;
			color: #777;
			margin: 30rpx 0;
		}
		.content{
			padding-bottom: 50rpx;
		}
		.copyRight{
			background: #FEF0F0;
			font-size: 26rpx;
			padding:20rpx;
			color:#F89898;
			line-height: 1.8em;
		}
	}

</style>
