<template>
	<view class="body">
		<view class="bujki">
			<view class="item-masonry name" v-for="(item, index) in list" :key="index" @click="xz(item.id)">
				<view :class="lx==item.id ? 'yasnhi':''">{{item.title}}</view>
				
			</view>
		</view>
		<view class="bujki-1">
			<view v-if="comList.length>0" >
				<view class="item-masonry name" v-for="(item, index) in comList" :key="index" @click="Jump(item.id)">
					<view>
						<image class="images" :src="item.ioc" mode="widthFix"></image>
					</view>
					<view class="title">
						{{item.title}}
					</view>
					
				</view>
			</view>
			<view v-if="comList.length==0">
				<u-empty
				        mode="search"
				        icon="http://shejiaoimg.oss-cn-beijing.aliyuncs.com/Img/42.png"
				>
				</u-empty>
			</view>
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				lx:0,
				uid:0,
				list: [
					{id:1,title:"基础知识"},
					{id:2,title:"智能组网"},
					{id:3,title:"智家业务"},
					{id:4,title:"全屋智能"},
					{id:5,title:"智慧营维APP"},
					{id:6,title:"服务礼仪"},
					{id:7,title:"业务随销"},
					{id:8,title:"其它"},
					],
				comList: [],
			}
		},
		onLoad(e) {
			uni.hideShareMenu({
			  menus: ['shareAppMessage', 'shareTimeline']
			})
			this.lx=e.id;
			this.getlb();
		},
		onShow() {
			
		},
		methods: {
			
			xz(id){
				this.lx=id;
				this.getlb();
			},
			getlb(){
				uni.$u.http.post('/Course/xcxLx', {fid:this.lx}).then(res => {
					this.comList=res.data
					
				}).catch(err => {})
				
			},
			Jump(id){
				console.log(id)
				
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
				
			},
			
		}
	}
	
</script>

<style lang="scss">
	.body{
		padding: 20rpx;
	}
	.bujki{
		width: 35vw;
		float: left;
	}
	.bujki-1{
		width: 57vw;
		float: left;
		padding-left: 2vw;
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
		padding: 20rpx;
		box-shadow: 0px 0px 28rpx 1rpx rgba(78, 101, 153, 0.14);
	}
	.yasnhi{
		color: #007AFF;
	}
	.images {
		width: 100%;
		border-radius: 10rpx;
	}
	.title {
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
		color: #666666;
	}
	
	.freu2{
		color: #ff0000;
		font-weight: 600;
		line-height: 60rpx;
	}
	.freu3{
		color: #007AFF;
		// font-weight: 600;
		margin-left: 20rpx;
		line-height: 60rpx;
	}
</style>
