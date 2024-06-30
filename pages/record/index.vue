<template>
	<view class="user">
		<view class="com-item">
			<view class="com-wrap">
				<view class="cell" v-for="(item, index) in serverList" :key="index" @click="xqa('../examination/index?id='+item.id+'&lx=1')">
					<view class="cell-left">
						<view class="cell-text">{{ item.title }}-{{item.ctime}}</view>
					</view>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
	
		data() {
			return {
				uid: 0,
				serverList: []
			};
		},
		onShow() {
			let uid = uni.getStorageSync('uid');
			if (uid) {
				this.uid=uid;
			}
			this.getlist();
		},
		methods: {
			getlist(){
				uni.$u.http.post('PaperTitleSubjectUser/kaoshilist', {
						"uid": this.uid
					})
					.then(res => {
						this.serverList=res.data
					console.log(res)	
					})
			},
			xqa(v){
				uni.navigateTo({
					url:v
				})
			}
			
			
	
		}
	};
</script>

<style lang="scss">
	page {
		background: #f2f2f2;
	}
	.user {
		.user-wrap {
			display: flex;
			justify-content: center;
			align-items: center;
			height: 50vw;
			padding: 30rpx;
			z-index: 9;
			border-radius: 0 0 20% 20%;
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
</style>
