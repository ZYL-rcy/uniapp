<template>
	<view class="container">
		<view class="hello">HELLO!</view>
		<view v-if="loadtype==='dl'">
			<view class="item">
				<view class="label">
					<text>邮箱: </text>
				</view>
				<input type="text" value="" v-model="username" />
			</view>

			<view class="item">
				<view class="label">
					<text>密码:
					</text>
				</view>
				<input type="password" value="" v-model="password" />
			</view>
			
			<a class="forgot-password" href="#">忘记密码?</a>
			<view class="button-container">

				<button class="login-button" type="default" @tap="checklogin">登录</button>
				<button class="register-button" type="default" @tap="registerclick">注册</button>
				
			</view>
		</view>
		<view v-if="loadtype==='zc'">
			<view class="item">
				<view class="label">
					<text space="ensp">邮    箱： </text>
				</view>
				<input type="text" value="" v-model="username" />
			</view>
			<view class="item">
				<view class="label">
					<text space="ensp">密    码：</text>
				</view>
				<input type="password" value="" v-model="password" />
			</view>
			<view class="item">
				<view class="label">
					<text>确认密码：</text>
				</view>
				<input type="password" value="" v-model="confirmpassword" />
			</view>

			<view class="item">
				<view class="label">
					<text space="ensp">验 证 码：</text>
				</view>
				<view class="input-wrapper">
					<input type="password" class="yzm-btn-input" value="" v-model="captcha" />
					<button type="default" class="yzm-btn" v-bind:disabled="btndisabled"
						@tap='getyzm'>{{btnstr}}</button>
				</view>
			</view>
			<view class="button-container">

				<button class="login-button" type="default" @tap="loginclick">返回</button>
				<button class="register-button" type="default" @tap="registerclic">注册</button>

			</view>
		</view>

		<view class="footer">
			<view class="checkbox-label">
				<text>我已阅读并同意用户协议</text>
			</view>
		</view>
	</view>

</template>
<script>
	export default {
		data() {
			return {
				username: '',
				password: '',
				captcha: '',
				cookie: '',
				confirmpassword: '',
				vcode: '',
				vcodesrc: this.url + 'login/vcode',
				loadtype: 'dl',
				yzmstr: '获取验证码',
				btnstr: '获取验证码',
				btndisabled: false
			}
		},
		onLoad() {

		},
		methods: {
			timewait(t) {
				const _this = this
				setTimeout(function() {
					if (t >= 0) {
						_this.btnstr = t + '秒后点击';
						t--;
						_this.timewait(t);
					} else {
						_this.btnstr = '获取验证码';
						t = 60;
						_this.btndisabled = false
					}
				}, 1000)
			},
			//获取验证码
			getyzm() {
				uni.request({
					url: 'https://openapi.hackerxiao.online/api/v1/auth/sendemail?email=' + this.username +
						'&mode=sign',
					method: 'POST',

					data: {
						email: '1663714611@qq.com',
						mode: 'sign'
					},
					header: {
						'content-type': 'application/x-www-form-urlencoded'
					},
					success: res => {
						if(res.data.msg='邮箱已被注册'){
							uni.showToast({
								title:'此邮箱已被注册',
								icon:'error',
							})
							
							
							
						}
						
						console.log(res.data);
					},
					fail: () => {},
					complete: () => {}
				});

				this.timewait(60);
				this.btndisabled = true;
			},
			//注册接口
			registerclic() {
				if(!this.code){
					uni.showToast({
						duration: 1500,
						title: '请输入验证码',
						mask: true,
						icon: 'none'
					})
					return;
				}
				if(!this.email){
					uni.showToast({
						duration: 1500,
						title: '请输入邮箱',
						mask: true,
						icon: 'none'
					})
					return;
				}
				uni.showLoading({
					mask: true
				})
				uni.request({
					url: 'https://openapi.hackerxiao.online/api/v1/auth/signup',
					method: 'POST',
					data: {
						captcha: this.captcha,
						Email: this.username,
						password: this.password,
						repassword: this.confirmpassword,
						username: 'ZYL',
					},
					success: res => {
						console.log(res)
						
						if (res.data.msg == 'success') {
							uni.showToast({
								title: '注册成功',
								icon: 'success',

							});
							console.log(3);
							//跳转到每周计划界;面，适合taBar中的界面
							uni.switchTab({
								url: '/pages/plan/plan'
							});
						}
					},
					fail: () => {},
					complete: () => {}
				})
			},
			loginclick() {
				this.loadtype = 'dl'
			},
			registerclick() {
				this.loadtype = 'zc'
			},
			getvcode() {
				this.vcodesrc = this.url + 'login/vcode/' + Math.random();
			},
			checklogin() {
				// 登录接口
				uni.request({
					url: 'https://openapi.hackerxiao.online/api/v1/auth/login',
					method: 'POST',
					data: {
						Email: this.username,
						Password: this.password
					},
					success: res => {
						console.log(res.data.data.token);
						if (res.data.msg === 'success') {
							uni.showToast({
								title: '登陆成功',
								icon: 'success'
							});

							uni.setStorage({
								key: 'token',
								data: res.data.data.token,
								success: () => {
									console.log('Token saved successfully');

								}
							});

							// 跳转到每周计划界面，适合taBar中的界面
							
							uni.switchTab({  
							            url: '/pages/index/index',  
							            success(e) {  
							                console.log(e);  
							            },  
							            fail(e) {  
							                console.log(e);  
							            }  
							        });  
						} else {
							uni.showToast({
								title: '用户名或密码错误',
								icon: 'none'
							});
							console.log(1);
						}
					},
					fail: () => {},
					complete: () => {}
				});
			},

			//绑定教务
			bindApiRequest() {
				uni.getStorage({
					key: 'token',
					success: function(res) {
						const token = res.data;
						// 绑定接口
						uni.request({
							url: 'https://openapi.hackerxiao.online/api/v1/edu/bind',
							method: 'POST',
							header: {
								Authorization: 'Bearer ' + token,
								'Content-Type': 'application/json',
							},
							data: {
								password: 'Zyl051811',
								studentID: '2203050232'
							},
							success: res => {

								console.log('Request successful');
								console.log(res);
							}
						});

					}
				});
			},
			//获取cookie
			 getcook() {
				uni.getStorage({
					key: 'token',
					success: function(res) {
						const token = res.data;
						uni.request({
							url: 'https://openapi.hackerxiao.online/api/v1/edu/cookie',
							method: 'GET',
							header: {
								Authorization: 'Bearer ' + token,
								'Content-Type': 'application/json',
							},
							data: {

							},
							success: (res) => {
								uni.setStorage({
									key: 'cookie',
									data: res.data.data,
									success: () => {
										console.log('cookie saved successfully');

									}
								});
								console.log(res.data.data);
								
							}
						});
					}
				});
			},
			//获取课表
			getkebiao() {
				uni.getStorage({
					key: 'token',
					success: function(res) {
						const token = res.data;
						 uni.getStorage({
						 	key: 'cookie',
							success: function(res) {
								const cookie = res.data;
								uni.request({
									url: 'https://openapi.hackerxiao.online/api/v1/edu/courses',
									method: 'POST',
									header: {
										Authorization: 'Bearer ' + token,
										'Content-Type': 'application/json',
										
									},
									data: {
										cookie: cookie,
										semester: 12,
										year: "2022"
									},
									success: (res) => {
									
										uni.setStorage({
											key: 'classid',
											data: res.data.data.classid,
											success: () => {
												console.log('cookie saved successfully');
										
											}
										});
										console.log(res);
										console.log(cookie);
										
									}
								});
							},
						});
				 	}
				});
			},
			//获取成绩
			getgrades(){
				
				uni.getStorage({
					key: 'token',
					success: function(res) {
						const token = res.data;
						 uni.getStorage({
						 	key: 'cookie',
							success: function(res) {
								const cookie = res.data;
								uni.request({
									url: 'https://openapi.hackerxiao.online/api/v1/edu/grades',
									method: 'POST',
									header: {
										Authorization: 'Bearer ' + token,
										'Content-Type': 'application/json',
										
									},
									data: {
										cookie: cookie,
										semester: 12,
										year: "2022"
									},
									success: (res) => {
										
										console.log(res);
										console.log(cookie);
										console.log(token);
										for(var i=0;i<5;i++){
										console.log(res.data.data[i].classid);
										}
									}
								});
							},
						});
				 	}
				});
			}

		},
		//获取详细成绩
	}
</script>
<style scoped>
	.input-wrapper {
		position: relative;
	}

	.yzm-btn-input {
		margin-right: 10px;
	}

	.yzm-btn {
		position: absolute;
		font-size: 13px;
		right: 12px;
		top: 5px;
		/* padding: 5px 10px; */
	}

	.container {
		display: flex;
		flex-direction: column;
		align-items: center;
		margin-top: 100px;

	}

	.item {
		display: flex;
		align-items: center;
		margin-bottom: 20px;
		animation: slideUp 0.75s ease-out;
	}

	@keyframes slideUp {
		0% {
			opacity: 0;
			transform: translateY(20px);
		}

		100% {
			opacity: 1;
			transform: translateY(0);
		}
	}

	.label {
		margin-right: 10px;
		font-weight: bold;
	}

	input {
		padding: 10px;
		border: none;
		border-radius: 5px;
		background-color: #f5f5f5;
		width: 200px;
		transition: background-color 0.5s;
		box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);
	}

	input:focus {
		background-color: #fff;
		outline: none;
		box-shadow: 0 0 5px rgba(0, 0, 0, 0.2);
	}

	.button-container {
		display: flex;
		justify-content: space-between;
		width: 300px;
	}

	.login-button,
	.register-button {
		padding: 5px 20px;
		/* 调整按钮的padding */
		border-radius: 5px;
		background-color: #a3b7a0;
		color: #fff;
		border: none;
		font-size: 12px;
		/* 调整按钮的字体大小 */
		font-weight: bold;
		text-transform: uppercase;
		letter-spacing: 1px;
		transition: background-color 0.3s, transform 0.3s, box-shadow 0.3s;
		box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);
		cursor: pointer;
		box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);
		margin-top: 35px;
	}

	.login-button:hover,
	.register-button:hover {
		background-color: #658f69;
		transform: scale(1.05);
		box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
	}

	.forgot-password {
		margin-top: -10px;
		margin-left: 103px;
		/* 将“忘记密码”往右移动10像素 */
		align-self: flex-start;
		color: #999;
		text-decoration: none;
		font-size: 12px;
		transition: color 0.3s;
	}

	.forgot-password:hover {
		color: #555;
	}

	.hello {
		font-family: 'Helvetica', sans-serif;
		font-size: 60px;
		color: #333;
		margin-left: 40px;
		margin-top: -10px;
	}

	.footer {
		display: flex;
		justify-content: center;
		align-items: center;
		margin-top: 20px;

	}

	.checkbox-label {
		display: flex;
		align-items: center;
		margin-top: 280px;
		align-self: flex-start;
		color: #999;
		text-decoration: none;
		font-size: 12px;
		transition: color 0.3s;
	}
</style>