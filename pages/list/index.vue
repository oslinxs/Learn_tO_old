<template>
	<view>
		<view class="pubuBox">
			<u-search placeholder="请输入关键字"  v-memo="title" @change="change" @search="chaxun" @custom="chaxun"></u-search>
			
		</view>
		<u-line></u-line>
		<view>
			<u-tabs :list="list" @click="click" :current="current"></u-tabs>
		</view>
		<view>
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
		

	</view>
</template>

<script>
	export default {
		data() {
			return {
				current:2, 
				show: false,
				title:"",
				page:1,
				list: [{
					name: '综合排序',
				}, {
					name: '热度排序',
				}, {
					name: '最新课程'
				}],
				comList: []
			}
		},
		onLoad() {
			uni.hideShareMenu({
			  menus: ['shareAppMessage', 'shareTimeline']
			})
		},
		onShow() {
			this.getlist()
		},
		methods: {
			getlist(){
				let data={
					title:this.title,
					page:this.page,
					limit:10,
					px:this.current,
				}
				uni.$u.http.post('/Course/xcxlist', data).then(res => {
					// this.comList.concat(res.data);
					console.log(res)
					
					if(res.data.length>0){
						for(let item in res.data){
							this.comList.push(res.data[item])
						}
						this.page=this.page+1
					}
				}).catch(err => {})
			},
			click(item) {
				this.current=item.index;
				this.comList=[];
				this.page=1;
				this.getlist();
				console.log('item', item);
			},
			change(v){
				this.title=v
				// console.log("111",v)
			},
			chaxun(){
				this.comList=[];
				this.page=1;
				this.getlist();
			},
			onReachBottom(){
				
				this.getlist(); 
				console.log('我滚动到底部了')
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
		},
		
	}
</script>

<style lang="scss">
	
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

	.kecheng {
		padding: 20rpx;
	}
</style>
