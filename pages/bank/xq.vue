<template>
	<view>
		<view class="body" >
			<u-row>
				<u-col span="4">
					<view>
						<image class="images" :src="datalist.ioc" mode="widthFix"></image>
					</view>
				</u-col>
				<u-col span="8">
					<view class="title">
						{{datalist.title}}
					</view>
					<view class="f_title">
						{{datalist.f_title}}
					</view>
					<view class="f_title">
						{{datalist.ctime}}更新
					</view>
					<view class="freu" v-if="datalist.pay==0">
						免费
					</view>
					<view class="freu2" v-if="datalist.pay>0">
						￥{{datalist.pay}}
					</view>
				</u-col>

			</u-row>
			
		</view>
		
		
		<view class="dibu" v-if="index==0">
			<u-row customStyle="margin-bottom: 10px">
				
				<u-col span="12">
					<view class="goumai" v-if="datalist.pay>0" @click="getsc()">立即购买</view>
					<view class="goumai2" v-if="datalist.pay==0" @click="jiru()">立即加入</view>
				</u-col>
			</u-row>
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
		onLoad(e) {
			uni.hideShareMenu({
			  menus: ['shareAppMessage', 'shareTimeline']
			})
			console.log(e)
			this.id=e.id;
			let uid=uni.getStorageSync('uid');
			this.datalist=uni.getStorageSync('ke_item');
			if(uid){
				this.uid=uid;
			}
			// this.getlist()
		},
		methods:{
			getlist(){
				uni.$u.http.post('/Course/byData', {id:this.id,uid:this.uid}).then(res => {
					// console.log(res)
					this.datalist=res.data.course;
					this.mulist=res.data.list;
					this.sc=res.data.sc;
					this.gm=res.data.gm;
				}).catch(err => {})
			},
			
			
			getsc(){
				if(this.uid==0){
					uni.showToast({
						title:"您还未登录！" ,
						icon:"none"
					})
				}else {
					
					uni.$u.http.post('/Pay/getSign', {id:this.datalist.id,uid:this.uid,lx:1}).then(res => {
						// console.log(res)
						uni.requestPayment({
							timeStamp: res.timeStamp,
							nonceStr: res.nonceStr,
							package: res.package,
							signType: res.signType,
							paySign: res.paySign,
							success: (res) => {
								uni.showToast({
									title: "支付成功!"
								})
							},
						
							fail: (res) => {
								uni.showModal({
									content: "支付失败,原因为: " + res.errMsg,
									showCancel: false
								})
							
							}
						})
						
					}).catch(err => {})	
					
				}
				
			},
			jiru(){
				if(this.uid==0){
					uni.showToast({
						title:"您还未登录！" ,
						icon:"none"
					})
				}else {
					
				
					uni.$u.http.post('/PaperUser/add', {paperid:this.datalist.id,uid:this.uid}).then(res => {
						// console.log(res)
						if(res.code==200){
							uni.showToast({
								title:'加入成功！'
							})
							setTimeout(function() {
								let pages = getCurrentPages(); // 当前页面
								let beforePage = pages[pages.length - 2]; // 上一页
								uni.navigateBack({
									success: function() {
										beforePage(); // 执行上一页的onLoad方法
									}
								});
							}, 1000);
						}else{
							uni.showModal({
								content:res.msg,
								showCancel: false
							})
						}
						
					}).catch(err => {})
				}
			}
			
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

	.images {
		width: 100%;
		border-radius: 10rpx;
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

	.freu {
		color: #53C21D;
		font-weight: 600;
		line-height: 60rpx;
	}
	.freu2{
		color: #ff0000;
		font-weight: 600;
		line-height: 60rpx;
	}
	.jairu {
		color: #6e6e6f;
		font-size: 26rpx;
		line-height: 40rpx;

	}
	.u-content{
		padding: 24rpx;
	}
	.mulv{
		padding-bottom: 20rpx;
	}
	.nrecs{
		font-size: 30rpx;
		color: #484545;
		line-height: 50rpx;
		text-indent:2em;
		letter-spacing:4rpx;
	}
	.dibu{
		position:fixed;  
		right:5%; 
		left: 5%;
		bottom: 30rpx;
		background-color: #fff;
		height: 80rpx;
		border-radius: 10rpx;
		box-shadow: 10rpx 10rpx 3rpx  rgba(78, 101, 153, 0.2);
	}
	.goumai{
		background-color: #2196F3;
		text-align: center;
		color: #fff;
		line-height: 80rpx;
		border-top-right-radius: 10rpx;
		border-bottom-right-radius: 10rpx;
	}
	.goumai2{
		background-color: #53C21D;
		text-align: center;
		color: #fff;
		line-height: 80rpx;
		border-top-right-radius: 10rpx;
		border-bottom-right-radius: 10rpx;
	}
	.juzhong{
		padding: 10rpx 20rpx;
	}
</style>
