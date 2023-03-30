<template>
	<view class="out" @click="navTo(item)">
		
		<image :src="item.picurl" mode="aspectFill"></image>
		
		<view class="text">
			<view class="title">{{item.title}}</view>
			<view class="info" v-if="!item.looktime">
				<text>{{item.author}}</text>
				<text>{{item.hits}}浏览量</text>
			</view>
			<view class="info" v-else>
				<text>浏览时间：{{item.looktime}}</text>
			</view>
		</view>
		
	</view>
</template>

<script>
	export default {
		name:"newsItem",
		props: {
			item: {
				type: Object,
				default(){
					return{
						title:"默认标题",
						author:"作者",
						hits:0,
						picurl:"../../static/images/img1.png"
					}
				}
			},
		},
		data() {
			return {

			};
		},
		methods:{
			navTo(e){
				uni.navigateTo({
					url:`/pages/detail/detail?cid=${e.classid}&id=${e.id}`
				})
			}
		}
	}
</script>

<style lang="scss">
	.out{
		display: flex;
		image{
			width: 230rpx;
			height: 160rpx;
		}
		.text{
			padding-left: 20rpx;
			flex: 1;
			display: flex;
			flex-direction: column;
			justify-content: space-between;
			.title{
				font-size: 37rpx;
				overflow: hidden;
				-webkit-line-clamp: 2;
				text-overflow: ellipsis;
				display: -webkit-box;
				-webkit-box-orient: vertical;
		}
			.info{
				font-size: 27rpx;
				color: #999;
				text{
					padding-right: 30rpx;
				}
			}
		}
	}
</style>