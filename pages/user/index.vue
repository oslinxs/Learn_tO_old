<template>
	<view class="user">
		<!-- 头部 -->
		<view class="user-wrap">
			<view class="setting iconfont icon31shezhi"></view>
			<view class="info" v-if="userInfo && !userInfo.logo">
				<image class="avatar" mode="aspectFill" src="https://img0.baidu.com/it/u=4142899193,1027177229&fm=253&fmt=auto&app=120&f=JPEG?w=650&h=475"></image>
				<view class="nickname">{{ userInfo.name }}</view>
			</view>
			<view class="info" v-if="userInfo && userInfo.logo">
				<image class="avatar" mode="aspectFill" :src="userInfo.logo"></image>
				<view class="nickname">{{ userInfo.name }}</view>
			</view>
			<view class="info" v-if="!userInfo">
				<view  @click="xqa('../login/index')" class="denglnui">用户登录
				</view>
			</view>
		</view>

		

		<view class="com-item" v-if="userInfo && !userInfo.openid">
			<view class="com-wrap">
				<view class="cell" >
					<view class="cell-left">
						<image class="cell-icon" src="/static/images/user/icon-foot.png" mode="aspectFill"></image>
						<view class="cell-text"><button open-type="getUserProfile" lang="zh_CN" @tap="wxGetUserInfo">绑定微信</button></view>
					</view>
					<view class="iconfont iconmore1"></view>
				</view>
			</view>
		</view>
		
		<view class="com-item" v-if="userInfo && userInfo.openid && cid==0">
			<view class="com-wrap">
				<view class="cell" >
					<view class="cell-left">
						<image class="cell-icon" src="/static/images/user/xiao.png" mode="aspectFill"></image>
						<view class="cell-text" @click="ding()">订阅消息</view>
					</view>
					<view class="iconfont iconmore1"></view>
				</view>
			</view>
		</view>

		<!-- 用户服务 -->
		<view class="com-item" v-if="userInfo">
			<view class="com-wrap">
				<view class="cell" v-for="(item, index) in orderStatusList" :key="index" @click="xqa(item.path)">
					<view class="cell-left">
						<image class="cell-icon" :src="item.icon" mode="aspectFill"></image>
						<view class="cell-text">{{ item.title }}</view>
					</view>
					<view class="iconfont iconmore1"></view>
				</view>
			</view>
		</view>
		<view class="com-item" v-if="userInfo">
			<view class="com-wrap">
				<view class="cell" v-for="(item, index) in serverList" :key="index" @click="xqa(item.path)">
					<view class="cell-left">
						<image class="cell-icon" :src="item.icon" mode="aspectFill"></image>
						<view class="cell-text">{{ item.title }}</view>
					</view>
					<view class="iconfont iconmore1"></view>
				</view>
			</view>
		</view>
		
	</view>
</template>

<script>
	export default {

		data() {
			return {
				userInfo: null,
				uid: 0,
				cid:0,
				xcxcode: '', //获取登录用户的code
				orderStatusList: [{
						title: '个人信息',
						icon: '/static/images/user/uesr.png',
						path: '../sign/index'
					},{
						title: '答题记录',
						icon: '/static/images/user/icon-foot.png',
						path: '../mycourse/index'
					},
					{
						title: '消息通知',
						icon: '/static/images/user/xiao.png',
						path: '../question/index'
					}
				],
				currentIndex: 0,
				serverList: [{
						title: '客服中心',
						icon: '/static/images/user/icon-kefu.png',
						path: '../xq/index?id=3'
					},
					{
						title: '用户退出',
						icon: '/static/images/user/icon-help.png',
					}
				]
			};
		},
		onLoad() {
			uni.hideShareMenu({
			  menus: ['shareAppMessage', 'shareTimeline']
			})
		},
		onShow() {
			this.login();
			let linghuo = uni.getStorageSync('linghuo');
			if(linghuo){
				this.cid=1;
			}
			let xcxUserData = uni.getStorageSync('xcxUserData');
			if(xcxUserData){
				this.userInfo=xcxUserData;
				this.userData();
				console.log(22)
				if(xcxUserData.openid){
					console.log(11)
					if(this.cid==0){
						this.ding()
					}
				}
			}
			
			// this.ding()

		},
		methods: {
			Jom(e) {
				console.log(e)
			},
			ding(){
				let than=this;
				// console.log(1)
				uni.requestSubscribeMessage({
				  tmplIds: ['PCNyiqexS2VJ7tLl6FSc3xIYkvNk29AAhy_QPfLCol8'],// 
				  success (res) {
					// console.log("res",res)
					if(res.PCNyiqexS2VJ7tLl6FSc3xIYkvNk29AAhy_QPfLCol8=="accept"){
						uni.setStorageSync("linghuo", true);
						than.cid=1;
					}
					
				  }
				})
						
			},
			//获取用户信息
			wxGetUserInfo() {
				let _self = this;
				uni.getUserProfile({
					provider: 'weixin',
					desc: '获取用户完整信息',
					success: (infoRes) => {
						console.log(infoRes.userInfo)

						//提示
						uni.showLoading({
							title: '绑定中',
							mask: true
						});
						// 2.提交数据到后台
						uni.$u.http.post('User/getOpenid', {
								"uid":_self.userInfo.id,
								"code": _self.xcxcode,
								"logo": infoRes.userInfo.avatarUrl
							})
							.then(res => {
								console.log(res)
								_self.userData()

							})
					},
					fail(res) {
						uni.showToast({
							title: '获取用户信息失败',
							icon: 'none'
						});
					}
				});

				return false
			},
			//登录
			login() {
				let _self = this;
				// 1. wx 获取登录用户 code
				uni.login({
					provider: 'weixin',
					success: loginRes => {
						_self.xcxcode = loginRes.code;
					},
					fail: () => {
						return false;
					}
				});
				return false;
			},
			userData() {
				uni.$u.http.post('User/byData', {
						"id": this.userInfo.id
					})
					.then(res => {
						if(res.data.aid==0){
							this.userInfo=null;
							uni.clearStorageSync();
							
							uni.showToast({
								title:'禁止使用小程序'
							})
						}else{
							uni.setStorageSync("xcxUserData", res.data);
							this.userInfo = res.data;;
						}
						
						setTimeout(function() {
							uni.hideLoading();
						}, 100);


					})
			},
			xqa(v){
				if(v){
					uni.navigateTo({
						url:v
					})
				}else{
					this.userInfo=null;
					uni.clearStorageSync();

					uni.showToast({
						title:'退出成功'
					})
				}
				
			}

		}
	};
</script>

<style lang="scss">
	page {
		background: #f2f2f2;
	}

	.btn-hover {
		background: #f2f2f2 !important;
	}

	.user {
		.user-wrap {
			display: flex;
			justify-content: center;
			align-items: center;
			height: 50vw;
			padding: 30rpx;
			z-index: 9;
			// border-radius: 0 0 20% 20%;
			border-radius: 0 0 10rpx 10rpx;
			background-color: #3d9bf6;
			// background: url('https://handsel.oss-cn-shenzhen.aliyuncs.com/1588938371592.jpg') no-repeat;
			background-size: cover;

			.setting {
				color: #fff;
				position: absolute;
				top: 60rpx;
				left: 60rpx;
				font-size: 50rpx;
			}

			.info {
				position: absolute;
				text-align: center;

				.avatar {
					width: 150rpx;
					height: 150rpx;
					border-radius: 50%;
				}

				.nickname {
					color: #fff;
					font-size: 28rpx;
				}
			}
		}

		.order-status {
			padding: 0 20rpx;
			margin-top: -10vw;

			.status-wrap {
				border-radius: 25rpx;
				overflow: hidden;

				.status-list {
					display: flex;
					justify-content: space-evenly;
					align-items: center;
					background: #fff;
					padding-top: 30rpx;
					padding-bottom: 30rpx;

					.status-item {
						flex: 1;
						display: flex;
						flex-direction: column;
						justify-content: center;
						align-items: center;

						.item-icon {
							line-height: 1;
							font-size: 65rpx;
							color: #bbb;
						}

						.item-text {
							font-size: 28rpx;
							color: #666;
							margin-top: 5rpx;
						}
					}
				}
			}
		}

		.com-item {
			padding-left: 20rpx;
			padding-right: 20rpx;
			margin-top: 20rpx;
			

			.com-wrap {
				border-radius: 25rpx;
				overflow: hidden;
			}
		}

		.cell {
			height: 80rpx;
			padding-left: 20rpx;
			padding-right: 20rpx;
			display: flex;
			justify-content: space-between;
			align-items: center;
			background: #fff;
			border-bottom: 1px solid #f8f8f8;

			&:active {
				background: #f2f2f2;
			}

			&:last-child {
				border-bottom: none !important;
			}

			.cell-left {
				display: flex;
				align-items: center;

				.cell-icon {
					width: 50rpx;
					height: 50rpx;
				}

				.cell-text {
					color: #666;
					font-size: 28rpx;
					margin-left: 20rpx;
				}
			}

			.iconfont {
				font-size: 40rpx;
				color: #999;
			}
		}
	}
	.denglnui{
		background-color: rgba(130, 200, 230, 0.95);
		color: #fff;
		font-size: 30rpx;
		line-height: 30rpx;
		padding: 20rpx 40rpx !important;
		box-shadow: 4rpx 4rpx 2rpx rgba(78, 101, 153, 0.4);
		border-radius: 5rpx;
	}
	button {
		background-color: #fff;
		// background-color:#53C21D;
		color: #666;
		font-size: 28rpx;
		line-height: 28rpx;
		width: 80vw;
		padding: 20rpx 0;
		text-align: left;
		// padding: 20rpx 40rpx !important;
	}

	button::after {
		border: none
	}
</style>
