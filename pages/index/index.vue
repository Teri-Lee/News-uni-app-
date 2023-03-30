<template>
	<view class="home">
		<scroll-view scroll-x="true" class="navScroll">
			<view class="item" :class="navId==item.id?'active':''" v-for="item in navList" 
			:key="item.id" @click="navClick(item.id)">
			{{item.classname}}
			</view>
		</scroll-view>
		
		<view class="content">
			<view class="row" v-for="item in newsList" :key="item.id">
				<newsItem :item="item"></newsItem>
			</view>
			
			<view class="loading">
				<view v-if="loading==1">没有更多数据~~</view>
			</view>

		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				navId:50,
				curPage:1,
				navList:[],
				newsList:[],
				loading:0
			}
		},
		onLoad() {
			this.getList(),
			this.getNews()
		},
		onReachBottom() {
			if(this.loading==1){return}
			this.curPage++,
			this.getNews()
		},
		methods: {
			navClick(e){
				this.navId=e,
				this.curPage=1,
				this.loading=0,
				this.newsList=[],
				this.getNews()
			},
			
			getList(){
				uni.request({
					url:"https://ku.qingnian8.com/dataApi/news/navlist.php",
					success: (res) => {
						this.navList=res.data
					}
				})
			},
			
			getNews(){
				uni.showLoading({
					title:'加载中...',
					mask:true
				}),
				uni.request({
					url:"https://ku.qingnian8.com/dataApi/news/newslist.php",
					data:{
						cid:this.navId,
						page:this.curPage
					},
					success: (res) => {
						if(res.data.length<8){this.loading=1}
						this.newsList=[...this.newsList,...res.data],
						uni.hideLoading()
					}
				})
			}
		}
	}
</script>

<style lang="scss">
	.navScroll{
		height: 100rpx;
		background-color: #F7F7F7;
		font-size: 36rpx;
		white-space: nowrap;
		position: fixed;
		top: var(--window-top);
		left: 0;
		z-index: 10;
		.item{
			display: inline-block;
			line-height: 100rpx;
			padding: 0 50rpx;
			color: dimgrey;
			&.active{
				color: #68A5F7;
			}
		}
	}
	
	.content{
		padding: 20rpx;
		padding-top: 100rpx;
		.row{
			border-bottom:1px dashed #F7F7F7;
			padding: 20rpx 0;
		}
		.loading{
			text-align: center;
			font-size: 28rpx;
			color: #888;
			line-height: 2em;
		}
	}
</style>
