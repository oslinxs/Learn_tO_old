<!-- 蓝色登录页面2 -->
<template>
	<view>
		<view class="img-a">
			<view class="t-b">
				Tide and time don't await anything!
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
						<text class="txt">验证码</text>
						<view class="yihan">
						<input type="password" name="password"  placeholder="请输入您的验证码" v-model="form.testcode" />
						<button @click="test_code()" :disabled="isButtonDisabled">{{buttonText}}</button>
						</view>
					</view>
					<button @tap="login()">log in </button>	
				</form>
				
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
					password: "",
					testcode:""
				},
				buttonText:"点击获取验证码",
				isButtonDisabled: false,
				password_sub:""
			};
		},
		onLoad() {
			uni.hideShareMenu({
			  menus: ['shareAppMessage', 'shareTimeline']
			})
		},
		onShow() {
		},
		methods: {
			test_code(){
				      this.isButtonDisabled = true;
				      this.buttonText = '请稍等...';
				
				      let countdown = 60;
				      const interval = setInterval(() => {
				        countdown--;
				        this.buttonText = `${countdown}秒后重新获取`;
				
				        if (countdown === 0) {
				          clearInterval(interval);
				          this.isButtonDisabled = false;
				          this.buttonText = '点击获取验证码';
				        }
				      }, 1000);
			},
			


			//当前登录按钮操作
			login() {
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
				uni.$u.http.post('/User/XcxSignIn', this.form).then(res => {
					if (res.code == 200) {
						console.log(res)
						uni.setStorageSync('xcxUserData', res.data);
						// uni.setStorageSync('z_user', res.data.z_user);
						uni.setStorageSync('username', this.form.username)
						uni.setStorageSync('password', this.form.password)
						// uni.setStorageSync('sin', 1)
						 uni.showToast({
							title: '登录成功',
							icon: 'success',
							duration: 800 
						})
						uni.switchTab({
							url:'../user/index'
						})
					} else {
						 uni.showToast({
							title: res.msg,
							icon: 'none'
						})
					}
					
				}).catch(err => {})
				
			},

		}
	};
</script>
<style>
	.yihan{
		display: flex;
	}
	.wy{
		
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
		margin-top: -50rpx;
		font-size: 28rpx;
		padding-top: 70rpx;
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
