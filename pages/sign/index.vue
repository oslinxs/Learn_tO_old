<template>
	<view class="user">
		<!-- 头部 -->
		<view class="user-wrap">
			<view class="setting iconfont icon31shezhi"></view>
			<view class="info" v-if="userInfo && !userInfo.logo">
				<image class="avatar" mode="aspectFill" src="https://img0.baidu.com/it/u=4142899193,1027177229&fm=253&fmt=auto&app=120&f=JPEG?w=650&h=475"></image>
			</view>
			<view class="info" v-if="userInfo && userInfo.logo">
				<image class="avatar" mode="aspectFill" :src="userInfo.logo"></image>
			</view>
		</view>
		<view class="com-item" >
			<view class="com-wrap">
				<view class="cell" >
					<view class="cell-left">
						<u-icon name="pushpin-fill" color="#2196f3d9" size="28"></u-icon>
						<view class="cell-text" >编号：2022000{{userInfo.id}}</view>
					</view>
					<view class="iconfont iconmore1"></view>
				</view>
			</view>
		</view>
		
		<view class="com-item" >
			<view class="com-wrap">
				<view class="cell" >
					<view class="cell-left">
						<u-icon name="weixin-circle-fill" color="#2196f3d9" size="28"></u-icon>
						<view class="cell-text" v-if="userInfo.openid">微信：已绑定</view>
						<view class="cell-text" v-if="!userInfo.openid">微信：未绑定</view>
					</view>
					<view class="iconfont iconmore1"></view>
				</view>
			</view>
		</view>
		<view class="com-item" >
			<view class="com-wrap">
				<view class="cell" >
					<view class="cell-left">
						<u-icon name="account-fill" color="#2196f3d9" size="28"></u-icon>
						<view class="cell-text" >姓名：{{userInfo.name}}</view>
					</view>
					<view class="iconfont iconmore1"></view>
				</view>
			</view>
		</view>
		<view class="com-item" >
			<view class="com-wrap">
				<view class="cell" >
					<view class="cell-left">
						<u-icon name="phone-fill" color="#2196f3d9" size="28"></u-icon>
						<view class="cell-text" >手机号：{{userInfo.telephone}}</view>
					</view>
					<view class="iconfont iconmore1"></view>
				</view>
			</view>
		</view>
		<view class="com-item" >
			<view class="com-wrap">
				<view class="cell" >
					<view class="cell-left">
						<u-icon name="map-fill" color="#2196f3d9" size="28"></u-icon>
						<view class="cell-text" >地市：{{userInfo.aname}}</view>
					</view>
					<view class="iconfont iconmore1"></view>
				</view>
			</view>
		</view>
		<view class="com-item" >
			<view class="com-wrap">
				<view class="cell" >
					<view class="cell-left">
						<u-icon name="map" color="#2196f3d9" size="28"></u-icon>
						<view class="cell-text" >区县：{{userInfo.bname}}</view>
					</view>
					<view class="iconfont iconmore1"></view>
				</view>
			</view>
		</view>
		<view class="com-item" >
			<view class="com-wrap">
				<view class="cell" >
					<view class="cell-left">
						<u-icon name="man-add-fill" color="#2196f3d9" size="28"></u-icon>
						<view class="cell-text" >认证等级：{{userInfo.cname}}</view>
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
				
			};
		},
		onLoad() {
			uni.hideShareMenu({
			  menus: ['shareAppMessage', 'shareTimeline']
			})
		},
		onShow() {
			let xcxUserData = uni.getStorageSync('xcxUserData');
			if(xcxUserData){
				this.userInfo=xcxUserData;
			}
			

		},
		methods: {
			
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
			height: 15vh;
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
	
</style>
