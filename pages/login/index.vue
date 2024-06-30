<!-- 蓝色登录页面2 -->
<template>
	<view>
		<view class="img-a">
			<view class="t-b">
				您好，
				<br />
				欢迎使用学习平台！
			</view>
		</view>
		<view class="login-view" style="">
			<view class="t-login">
				<form class="cl">
					<view class="t-a">
						<text class="txt">账号</text>
						<input type="text" name="username" placeholder="请输入您的账号"  v-model="form.username" />
					</view>
					<view class="t-a">
						<text class="txt">密码</text>
						<input type="password" name="password"  placeholder="请输入您的密码" v-model="form.password" />
					</view>
					<button @tap="login()">log in</button>
					<br/>
					<button @tap="enroll()">register </button>	
				</form>
				<br/>
				<br/>
				<text class="wy" @click="code()">验证码登录</text>
			</view>
		</view>
	</view>
</template>
<script>
	export default {
		data() {
			return {
				form: {
					username: "",
					password: ""
				},
				
			};
		},
		onLoad() {
			uni.hideShareMenu({
			  menus: ['shareAppMessage', 'shareTimeline']
			})
		},
		onShow() {
			let that = this;
			//页面加载完成，获取本地存储的用户名及密码
			const userName = uni.getStorageSync('username');
			const userPsw = uni.getStorageSync('password');
			if (userName && userPsw) {
				that.form.username = userName;
				that.form.password = userPsw;
			} else {
				that.form.username = "";
				that.form.password = "";
			}
		},
		methods: {

			code(){
				uni.navigateTo({
					url:"/pages/login/code_enroll"
				})
			},
			enroll() {
				uni.navigateTo({
					url:"/pages/login/enroll"
				});
			},
			
			//当前登录按钮操作
			login() {
				 console.log(this.form)
				var that = this;
				if (!that.form.username) {
					uni.showToast({
						title: '请输入您的账号',
						icon: 'none'
					});
					return;
				}
				if (!that.form.password) {
					uni.showToast({
						title: '请输入您的密码',
						icon: 'none'
					});
					return;
				}
				uni.request({
					url:"https://192.168.169.66:8081/user/login-by-password",
					method:'POST',
					 header: {
					          'content-type': 'application/json'
					        },
					data: {
					        username: this.form.username,
					        password: this.form.password
					      },
					success: (res) => {
				  if (res.statusCode === 200 && res.data.code === 200) {
				  // 登录成功，保存会话标识符（例如 token）
				  const token = res.data.token;
				  uni.setStorageSync('token', token);  // 将 token 保存在本地存储中
				  uni.showToast({
					title: '登录成功',
					icon: 'success'
				  });}else {
					            console.error('请求失败：', res);
					          }
					        },
					        fail: (err) => {
					          console.error('请求错误：', err);
					        }
				})
				
			},

		}
	};
</script>
<style>
	.wy{
		/* margin-top: 3000rpx; */
		color: #aaaaaa;
	}
	.txt {
		font-size: 32rpx;
		font-weight: bold;
		color: #333333;
	}

	.img-a {
		width: 100%;
		height: 450rpx;
		background-color: #2796f2;
	}

	.reg {
		font-size: 28rpx;
		color: #fff;
		height: 90rpx;
		line-height: 90rpx;
		border-radius: 50rpx;
		font-weight: bold;
		background: #f5f6fa;
		color: #000000;
		text-align: center;
		margin-top: 30rpx;
	}

	.login-view {
		width: 100%;
		position: relative;
		margin-top: -120rpx;
		background-color: #ffffff;
		border-radius: 8% 8% 0% 0;
	}

	.t-login {
		width: 600rpx;
		margin: 0 auto;
		font-size: 28rpx;
		padding-top: 80rpx;
	}

	.t-login button {
		font-size: 28rpx;
		background: #2796f2;
		color: #fff;
		height: 90rpx;
		line-height: 90rpx;
		border-radius: 50rpx;
		font-weight: bold;
	}

	.t-login input {
		height: 90rpx;
		line-height: 90rpx;
		margin-bottom: 50rpx;
		border-bottom: 1px solid #e9e9e9;
		font-size: 28rpx;
	}

	.t-login .t-a {
		position: relative;
	}

	.t-b {
		text-align: left;
		font-size: 42rpx;
		color: #ffffff;
		padding: 130rpx 0 0 70rpx;
		font-weight: bold;
		line-height: 70rpx;
	}

	.t-login .t-c {
		position: absolute;
		/* right: 22rpx; */
		top: 22rpx;
		background: #5677fc;
		color: #fff;
		font-size: 24rpx;
		border-radius: 50rpx;
		height: 50rpx;
		line-height: 50rpx;
		padding: 0 25rpx;
	}

	.t-login .t-d {
		text-align: center;
		color: #999;
		margin: 80rpx 0;
	}

	.t-login .t-e {
		text-align: center;
		width: 250rpx;
		margin: 80rpx auto 0;
	}

	.t-login .t-g {
		float: left;

		width: 50%;
	}

	.t-login .t-e image {
		width: 50rpx;
		height: 50rpx;
	}

	.t-login .t-f {
		text-align: center;
		margin: 150rpx 0 0 0;
		color: #666;
	}

	.t-login .t-f text {
		margin-left: 20rpx;
		color: #aaaaaa;
		font-size: 27rpx;
	}

	.t-login .uni-input-placeholder {
		color: #aeaeae;
	}

	.cl {
		zoom: 1;
	}

	.cl:after {
		clear: both;
		display: block;
		visibility: hidden;
		height: 0;
		content: '\20';
	}
</style>
