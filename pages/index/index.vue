<template>
	<view>
		<view class="u-demo-block">
			<u-swiper :list="list" previousMargin="20" nextMargin="20" circular :autoplay="true" interval="4000"
				radius="5" bgColor="#ffffff"></u-swiper>
		</view>
		<view>
			<u-grid :border="false" col="4">
				<u-grid-item v-for="(listItem,listIndex) in list1" :key="listIndex" @click="Jump('../bank/index?id='+listItem.id)">
					<u-icon :customStyle="{paddingTop:20+'rpx'}" :name="listItem.ioc" :size="35"></u-icon>
					<text class="grid-text">{{listItem.title}}</text>
				</u-grid-item>
			</u-grid>
			<!-- <u-toast ref="uToast" /> -->
		</view>
		
		<view class="znew">
			<u-row>
				<u-col span="9">
					<view class="title">最新知识库</view>
				</u-col>
				<u-col span="3">
					<view class="gengduo" @click="Jump('../list/index')">查看更多 ></view>
				</u-col>
			</u-row>
		</view>
		<view class="Index">
			<!-- 瀑布流布局列表 -->
			<view class="pubuBox">
				<view class="pubuItem">
					<view class="item-masonry" v-for="(item, index) in comList" :key="index" @click="xq(item.id)">
						<image :src="item.ioc" mode="widthFix"></image>
						<view class="listtitle">
							
							<view class="listtitle1">{{ item.title }}</view>
							
							<view class="listtitle3">
								更新时间：{{item.ctime |timestampToTimes }}
							</view>
						</view>
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
				list: [],
				list1: [
					{id:1,title:"基础知识",ioc:"http://frmimge.oss-cn-hangzhou.aliyuncs.com/xcx/1.png"},
					{id:2,title:"业务交付",ioc:"http://frmimge.oss-cn-hangzhou.aliyuncs.com/xcx/2.png"},
					{id:3,title:"服务礼仪",ioc:"http://frmimge.oss-cn-hangzhou.aliyuncs.com/xcx/3.png"},
					{id:4,title:"业务随销",ioc:"http://frmimge.oss-cn-hangzhou.aliyuncs.com/xcx/4.png"},
					{id:5,title:"智慧营维APP",ioc:"http://frmimge.oss-cn-hangzhou.aliyuncs.com/xcx/5.png"},
					{id:6,title:"智能产品",ioc:"http://frmimge.oss-cn-hangzhou.aliyuncs.com/xcx/6.png"},
					{id:7,title:"安全生产",ioc:"http://frmimge.oss-cn-hangzhou.aliyuncs.com/xcx/7.png"},
					{id:8,title:"其它",ioc:"http://frmimge.oss-cn-hangzhou.aliyuncs.com/xcx/8.png"}
				],
				comList: [],
			}
		},
		onLoad() {
			this.lb();
			uni.hideShareMenu({
			  menus: ['shareAppMessage', 'shareTimeline']
			})
		},
		onShow() {
			uni.removeStorageSync('ksitme');
		},
		methods: {
			lb(){
				uni.$u.http.post('/Operate/list', {lx:0}).then(res => {
					for(let item in res.data){
						this.list.push(res.data[item].ioc)
					}
				}).catch(err => {})
				uni.$u.http.post('/Course/xcxlist', {page:1,limit:10,px:2}).then(res => {
					this.comList=res.data;
					// console.log(res)
					// for(let item in res.data){
					// 	this.list.push(res.data[item].ioc)
					// }
				}).catch(err => {})
			},
			
			Jump(e){
				uni.navigateTo({
					url:e
				})
			},
			xq(id){
				let xcxUserData = uni.getStorageSync('xcxUserData');
				if(xcxUserData){
					uni.navigateTo({
						url:"../curriculum/index?id="+id
					})
				}else{
					uni.showToast({
						title: "用户未登录",
						icon: 'none'
					})
				}
				
			}
		}
	}
</script>

<style lang="scss">
	.u-demo-block {
		margin-top: 10rpx;
		flex: 1;
		margin-bottom: 10rpx;
	}

	.grid-text {
		font-size: 28rpx;
		color: #666;
		padding: 10rpx 0 20rpx 0rpx;
		/* #ifndef APP-PLUS */
		box-sizing: border-box;
		/* #endif */
	}

	.dianzhan {
		padding: 10rpx 20rpx;
	}

	.dianou {
		width: 100%;
		border-radius: 60rpx;
		height: 150rpx;

	}

	.znew {
		// width: 100%;
		margin: 40rpx 20rpx 5rpx 20rpx;
		border-left: 8rpx solid #2196F3;
	}

	.title {
		padding-left: 10rpx;
		font-size: 32rpx;
		font-weight: 600;
	}

	.gengduo {
		float: right;
		font-size: 25rpx;
		color: #7e7e80;
	}

	.tuijian {
		padding: 20rpx;
	}

	.goodsimg {
		width: 100%;
		height: 250rpx;
	}

	.pubuBox {
		padding: 22rpx;
	}

	.pubuItem {
		column-count: 2;
		column-gap: 20rpx;
	}

	.item-masonry {
		box-sizing: border-box;
		border-radius: 15rpx;
		overflow: hidden;
		background-color: #fff;
		break-inside: avoid;
		/*避免在元素内部插入分页符*/
		box-sizing: border-box;
		margin-bottom: 20rpx;
		box-shadow: 0px 0px 28rpx 1rpx rgba(78, 101, 153, 0.14);
	}

	.item-masonry image {
		width: 100%;
	}

	.listtitle {
		padding-left: 6rpx;
		font-size: 24rpx;
		padding-bottom: 22rpx;

		.listtitle1 {
			line-height: 39rpx;
			text-overflow: -o-ellipsis-lastline;
			overflow: hidden;
			text-overflow: ellipsis;
			display: -webkit-box;
			-webkit-line-clamp: 2;
			line-clamp: 2;
			-webkit-box-orient: vertical;
			min-height: 39rpx;
			max-height: 78rpx;
			font-size: 28rpx;
			color: #666;
		}

		.listtitle2 {
			color: #ff0000;
			font-size: 32rpx;
			line-height: 32rpx;
			font-weight: bold;
			padding-top: 10rpx;
			padding-bottom: 10rpx;

			.listtitle2son {
				font-size: 32rpx;
			}
		}
		.listtitle4 {
			color: #53C21D;
			font-size: 32rpx;
			line-height: 32rpx;
			font-weight: bold;
			padding-top: 10rpx;
			padding-bottom: 10rpx;
		
			.listtitle2son {
				font-size: 32rpx;
			}
		}

		.listtitle3 {
			font-size: 28rpx;
			color: #909399;
			line-height: 32rpx;
			padding-top: 10rpx;
		}
	}

	.Index {
		width: 100%;
		height: 100%;
	}
</style>
