<template>
	<view>
		<view class="item">
				<view class="">
					<text>用户名：</text>
				</view>
				<input type="text" value="" v-model="user_name" />
		</view>
		<view class="item">
				<view class="">
					<text>密码：</text>
				</view>
				<input type="text" value="" v-model="password"/>
		</view>
		<button type="default" @tap="finishRegister" >完成注册</button>
	</view> -
</template>

<script>
	export default {
		data() {
			return {
				user_name:"",
				password:""
			}
		},
		methods: {
			finishRegister(){
				
			}
		}
	}
</script>

<style>

</style>
