<template>
	<view>
		
		
		<view >
			<video id="myVideo" :src="datalist.aname" style="width: 100%;"></video>
		</view>
		<view class="ys" >
			<view class="title">{{datalist.title}}</view>
			<u-line margin="20rpx 0rpx"></u-line>
			<view class="title">发布时间：{{datalist.ctime}}</view>
		</view>
		<view class="body">
			<!-- <view>
				<u-tabs :list="list4" lineWidth="30" lineColor="#000" @click="dianji" :activeStyle="{
										color: '#303133',
										fontWeight: 'bold',
										transform: 'scale(1.05)'
									}" :inactiveStyle="{
										color: '#606266',
										transform: 'scale(1)'
									}" itemStyle="padding-left: 15px; padding-right: 15px; height: 34px;">
				</u-tabs>
			</view> -->
			<!-- <u-line></u-line> -->
			
			
			
			<view class="neir" >
				<view class="u-content nrecs">
					{{datalist.introduce}}
				</view>
			</view>
			
			
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				index: 0,
				lx:0,
				id:0,
				uid:0,
				gm:0,
				sc:0,
				datalist:{},
				mulist:[],
				video:"",
				list4: [{
					name: '介绍',
				}
				// , {
				// 	name: '目录'
				// }, {
				// 	name: '推荐'
				// }, {
				// 	name: '评价'
				// },
				],
			}
		},
		onLoad(e) {
			uni.hideShareMenu({
			  menus: ['shareAppMessage', 'shareTimeline']
			})
			this.id=e.id;
			
			this.getlist()
		},
		methods:{
			getlist(){
				uni.$u.http.post('/Course/byData', {id:this.id,uid:this.uid}).then(res => {
					console.log(res)
					this.datalist=res.data;
				}).catch(err => {})
			},
			
			
			
			
		}
	}
</script>

<style>
	page {
		background-color: #efefefdb;
	}

	.body {
		background-color: #FFF;
		border-radius: 10rpx;
		margin: 20rpx;
		padding: 20rpx;
	}

	

	.title {
		margin-left: 20rpx;
		white-space: nowrap;
		overflow: hidden;
		text-overflow: ellipsis;
	}

	.f_title {
		margin-left: 20rpx;
		color: #6e6e6f;
		font-size: 26rpx;
		line-height: 40rpx;
		word-break: break-all;
		text-overflow: ellipsis;
		overflow: hidden;
		display: -webkit-box;
		-webkit-line-clamp: 2;
		-webkit-box-orient: vertical;
	}

	
	.u-content{
		padding: 24rpx;
	}
	
	.nrecs{
		font-size: 26rpx;
		color: #484545;
		/* line-height: 50rpx; */
		/* text-indent:2em; */
		letter-spacing:4rpx;
	}
	
	
	
	
	.ys {
		background-color: #FFF;
		border-radius: 10rpx;
		margin: 20rpx;
		padding: 20rpx ;
		box-sizing: border-box;
		
		/*避免在元素内部插入分页符*/
		box-sizing: border-box;
		margin-bottom: 20rpx;
		/* box-shadow: 0px 0px 28rpx 1rpx rgba(78, 101, 153, 0.14); */
		line-height: 40rpx;
		font-size: 28rpx;
		color: #3e3e3e;
	}
	.neri{
		font-size: 28rpx;
		color: #3e3e3e;
	}
</style>
