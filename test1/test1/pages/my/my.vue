<template>
	<view>
		<button type="warn" @tap="gonavigate">navigateTo保留当前页，跳到应用内其他页</button>
		<view>
			------------------------------------
		</view>
		<button type="primary" @tap="goopenurl">openURL调用第三方程序打开指定的URL</button>
		<view>
			-------------------------------------
		</view>
		<button type="default" @tap="goregirect">redirectTo关闭当前页，跳转到应用内其他页</button>
	</view>
</template>

<script>
	export default{
		data(){
			return{
				
			}
		},
		methods:{
			gonavigate(){
				uni.navigateTo({
					url:'/pages/detail/detail'
				})
			},
			goregirect(){
				uni.redirectTo({
					url: '/pages/about/about'
				});

			},
			goopenurl(){
				void plus.runtime.openURL("http://www.baidu.com")
			}
		}
	}
</script>

<style>
</style>