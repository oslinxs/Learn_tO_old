<template>
	<view>
		<view>
			 <u-tabs :list="list1" @click="click" :current="current"></u-tabs>
		</view>
		<u-line></u-line>
		<view class="body" v-for="(item,index) in list" :key="index" @click="xq(item.id)">
			<view v-if="item.lx==2">{{item.title}}</view>
			<view v-if="item.lx==0">练习时间：{{item.stime}}</view>
			<view v-if="item.lx==1">模拟时间：{{item.stime}}</view>
			<view v-if="item.lx==2">考试开始时间：{{item.stime}}</view>
			<view>耗时：{{item.cid}}分钟</view>
			<view>答对：{{item.dadxmun+item.daddxmun+item.dafdmun}}道题</view>
			<view v-if="item.lx==2">总分：{{item.dadxfz+item.daddxfz+item.dafdfz}}</view>
		</view>
		
		<view v-if="list.length==0" class="neir" >
			
				<view class="u-content">
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
			current:0,
			id:0,
			uid:0,
			list1: [{
			   name: '专题练习',
			}, {
			   name: '模拟考试',
			}
			// , {
			//    name: '在线考试'
			// },
			
			],
			list:[],
			page:1,
		}
	},
	onLoad(e) {
		uni.hideShareMenu({
		  menus: ['shareAppMessage', 'shareTimeline']
		})
		let xcxUserData=uni.getStorageSync('xcxUserData');
		if(xcxUserData){
			this.uid=xcxUserData.id;
		}
		this.getlist()
	},
	methods:{
		click(item) {
			this.current=item.index;
			this.list=[];
			this.page=1;
			this.getlist()
		},
		getlist(){
			let data={
				lx:this.current,
				page:this.page,
				limit:10,
				uid:this.uid,
			}
			uni.$u.http.post('/Examination/uList', data).then(res => {
				// this.comList.concat(res.data);
				console.log(res)
				
				if(res.data.length>0){
					for(let item in res.data){
						this.list.push(res.data[item])
					}
					this.page=this.page+1
				}
			}).catch(err => {})
		},
		onReachBottom(){
			
			this.getlist(); 
			console.log('我滚动到底部了')
		},
		xq(id){
			uni.navigateTo({
				url:"../examination/index?id="+id
			})
		}
	}
}
</script>

<style>
	.body {
		background-color: #FFF;
		border-radius: 10rpx;
		margin: 20rpx;
		padding: 20rpx ;
		box-sizing: border-box;
		
		/*避免在元素内部插入分页符*/
		box-sizing: border-box;
		margin-bottom: 20rpx;
		box-shadow: 0px 0px 28rpx 1rpx rgba(78, 101, 153, 0.14);
		line-height: 40rpx;
		font-size: 28rpx;
		color: #666;
	}
	
	.u-content{
		padding: 24rpx;
	}
	.u-tabs__wrapper__nav__item__text {
	    font-size: 28rpx;
	    color: #666;
	}
</style>
