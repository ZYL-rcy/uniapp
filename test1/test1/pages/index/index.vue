<template>
	<view class="content">
		<view class="topbar">
			<view class="grade">成绩</view>
			<!-- <image src="https://static.21cake.com/themes/wap/img/logo.png" class="title-logo" alt="21cake logo"/> -->
			
		</view> 
		<view class="fill"></view>
		
		<view class="uni-flex uni-row info">
			<view class="flex-item">的撒</view>
			<view class="flex-item" style="margin: 0 10rpx">首页</view>
			<view class="flex-item">首页</view>
		</view>
		
	
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				
			}
		},
		onShow() {
			uni.showTabBar();
			
		},
		methods: {
			
			
		}
	}
</script>


<style lang="scss" scoped>
	.content {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
		.topbar{
			position: fixed;
			top: 0;
			left: 0;
			width: 100%;
			z-index: 10000;
			background: lightgreen;
			height: 48*2rpx;
			box-shadow: 0px 2px 4px 0px rgba(0,0,0,0.10);
			text-align: center;
				image{
					width:60rpx;
					height:60rpx;
					margin-top: 20rpx;
				}
			}
		.grade{
			width: 100%;
			height: -200px;
		}
		.fill{
			width: 100%;
			height:48*2rpx;
		}
	
		
	}
</style>
