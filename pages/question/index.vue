<template>
	<view>
		<view class="ys" v-for="(datalist,index) in mulist" :key="index" >
			
				<view class="title">{{datalist.ctime}} </view>
				<u-line margin="20rpx 0rpx"></u-line>
				<view class="neri">{{datalist.title}}</view>
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
			}
		},
		onLoad() {
			uni.hideShareMenu({
			  menus: ['shareAppMessage', 'shareTimeline']
			})
			this.getlist()
		},
		methods:{
			getlist(){
				uni.$u.http.post('/Message/list', {}).then(res => {
					console.log(res)
					this.mulist=res.data
				}).catch(err => {})
			},
			
			
		}
	}
</script>

<style>
	page {
		background-color: #efefefdb;
	}
	.ys{
		box-sizing: border-box;
		border-radius: 10rpx;
		margin: 20rpx;
		/*避免在元素内部插入分页符*/
		box-sizing: border-box;
		margin-bottom: 20rpx;
		box-shadow: 0px 0px 28rpx 1rpx rgba(78, 101, 153, 0.14);
		padding: 20rpx;
	}
	.title{
		color: #3e3e3e;
		font-size: 30rpx;
	}
	.neri{
		font-size: 28rpx;
		color: #3e3e3e;
	}
	
</style>
