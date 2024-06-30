<template>
	<view>
		<view class="hbody">


		</view>
		<view class="bbody">
			<u-grid :border="false" col="4">
				<u-grid-item v-for="(listItem,listIndex) in list" :key="listIndex" @click="xuanzhong">
					<u-icon :customStyle="{paddingTop:20+'rpx'}" :name="listItem.name" :size="30"></u-icon>
					<text class="grid-text" :class="idx==listIndex?'xhuan':''">{{listItem.title}}</text>
				</u-grid-item>
			</u-grid>
			<u-toast ref="uToast" />
		</view>
		<view class="cbody">
			<view class="com-item">
				<view class="com-wrap bicd" v-if="idx==0">
					<u--form labelPosition="left" :model="form"  ref="form1">
						
						<u-form-item label="选择题库" labelWidth="90" prop="form.sex" borderBottom @click="showSex = true; hideKeyboard()"
							ref="item1">
							<u--input v-model="form.tk"  disabled disabledColor="#ffffff" placeholder="请选择题库"
								border="none" ></u--input>
							<u-icon slot="right" name="arrow-right"></u-icon>
						</u-form-item>
						<u-form-item label="单项选择题" labelWidth="90" prop="form.dx" ref="item1">
							<u-number-box min="0" :max="dx" v-model="form.dx"></u-number-box>
						</u-form-item>
						<u-line></u-line>
						<u-form-item label="多项选择题" labelWidth="90" prop="form.ddx" ref="item1">
							<u-number-box min="0" :max="ddx" v-model="form.ddx" ></u-number-box>
						</u-form-item>
						<u-line></u-line>
						<u-form-item label="判断题" labelWidth="90" prop="form.fd" ref="item1">
							<u-number-box min="0" :max="fd" v-model="form.fd" ></u-number-box>
						</u-form-item>
						<u-line></u-line>
					</u--form>
					<view @click="submit" class="button">开始练习</view>
					<u-action-sheet
						:show="showSex"
						:actions="actions"
						title="请选择要练习的题库"
						@close="showSex = false"
						@select="sexSelect"
					>
					</u-action-sheet>
				</view>
				<view class="com-wrap" v-if="idx==2">
					<view class="cell" v-for="(item, index) in serverList" :key="index" @click="kaoshi(item)">
						<view class="cell-left">
							<view class="cell-text">{{index+1}}、{{ item.title }}</view>
							<!-- <view class="cell-text">考试时间：<se-spin class="cell-text-time">{{ item.stime }}至{{ item.etime }}</se-spin></view> -->
							<view class="cell-text">考试时间：<se-spin class="cell-text-time">{{ item.stime }}</se-spin></view>
							<view class="cell-text">考试时长：<se-spin class="cell-text-st">{{ item.ctime }}分钟</se-spin></view>
							<view class="cell-text">考试状态：<se-spin class="cell-text-st">{{ item.states==0 ? "未考试" : item.states==1 ? "考试中" : "已交卷"}}</se-spin></view>
						</view>
					</view>
					<view v-if="serverList.length==0">
						<u-empty mode="search" icon="http://shejiaoimg.oss-cn-beijing.aliyuncs.com/Img/42.png">
						</u-empty>
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
				showSex: false,
				idx: 0,
				shujuanid: 0,
				uid: 0,
				bydata: {},
				teda:[],
				ddx: 0,
				dx: 0,
				fd: 0,
				list: [{
						name: 'https://shejiaoimg.oss-cn-beijing.aliyuncs.com/Img/k1.png',
						title: '专题练习'
					},
					{
						name: 'https://shejiaoimg.oss-cn-beijing.aliyuncs.com/Img/k2.png',
						title: '模拟考试'
					},
					{
						name: 'https://shejiaoimg.oss-cn-beijing.aliyuncs.com/Img/k3.png',
						title: '在线考试'
					},
					{
						name: 'https://shejiaoimg.oss-cn-beijing.aliyuncs.com/Img/k4.png',
						title: '考试记录'
					}
				],
				serverList: [],
				form: {
					jid: 0,
					uid: 0,
					ddx: 0,
					dx: 0,
					fd: 0,
					tk:'全部',
					tid:0
				},
				actions: [{ name: '全部',id:"0" },
					{ name: '基础知识',id:"1"  },
					{ name: '业务交付',id:"2" },
					{ name: '服务礼仪',id:"3" },
					{ name: '业务随销',id:"4" },
					{ name: '智慧营维APP',id:"5" },
					{ name: '智能产品',id:"6" },
					{ name: '安全生产',id:"7" },
					{ name: '其它',id:"8" }
				]
			}
		},
		onLoad() {
			uni.hideShareMenu({
				menus: ['shareAppMessage', 'shareTimeline']
			})
			this.tmun()
		},
		onShow() {
			uni.removeStorageSync('ksitme');
			// let shujuanid = uni.getStorageSync('xz_item');
			// if (shujuanid) {
			// 	this.shujuanid = shujuanid;
			// }
			// let uid = uni.getStorageSync('uid');
			// if (uid) {
			// 	this.uid = uid;
			// }
			// this.idx = 0;
			// this.getlist();
			// this.getdata();
		},
		methods: {
			hideKeyboard() {
				uni.hideKeyboard()
			},
			sexSelect(e) {
				this.form.tk = e.name
				this.form.jid=e.id
				// this.$refs.form1.validateField('form.tk')
				this.tmun()
			},
			tmun(){
				uni.$u.http.post('/Examination/tcount', {
					lx: this.form.jid
				}).then(res => {
					// this.serverList=res.data
					console.log(res)
					this.ddx=res.data.ddx
					this.dx=res.data.dx
					this.fd=res.data.fd
					this.form.ddx=res.data.ddx
					this.form.dx=res.data.dx
					this.form.fd=res.data.fd
				
				}).catch(err => {})
			},
			xuanzhong(itme) {
				// console.log(itme)
				if (itme==0) {
					this.idx = 0;
					
				}else if (itme==1) {
					let xcxUserData = uni.getStorageSync('xcxUserData');
					if (xcxUserData) {
						uni.$u.http.post('/Examination/moni', {
							uid: xcxUserData.id
						}).then(res => {
							// this.serverList=res.data
							console.log(res)
							uni.redirectTo({
								url: "../examination/index?id=" + res.data
							})
						
						}).catch(err => {})
					}else{
						uni.showToast({
							title: '您还未登录',
							icon:'none',
							duration: 1000
						});
					}
				}else if (itme==2) {
					this.idx = 2;
					let xcxUserData = uni.getStorageSync('xcxUserData');
					if (xcxUserData) {
						uni.$u.http.post('/Examination/xcxlist', {
							uid: xcxUserData.id
						}).then(res => {
							this.serverList=res.data
						
						}).catch(err => {})
					}else{
						uni.showToast({
							title: '您还未登录',
							icon:'none',
							duration: 1000
						});
					}
				}else if (itme==3) {
					let xcxUserData = uni.getStorageSync('xcxUserData');
					if (xcxUserData) {
						uni.navigateTo({
									url: "../mycourse/index"
								})
					}else{
						uni.showToast({
							title: '您还未登录',
							icon:'none',
							duration: 1000
						});
					}
					
				}
				

			},
			Jump(e) {
				uni.navigateTo({
					url: e
				})
			},
			getlist() {
				uni.$u.http.post('/Paper/xcxhomelist', {
					id: this.shujuanid
				}).then(res => {
					this.bydata = res.data
					// console.log(res)

				}).catch(err => {})
			},

			getdata() {
				uni.$u.http.post('/PaperTitle/xcxlist', {
					id: this.shujuanid,
					lx: this.idx
				}).then(res => {
					this.serverList = res.data
					// console.log(res)

				}).catch(err => {})
			},
			kaoshi(itme) {
				console.log(itme)
				if(itme.states==2){
					uni.showToast({
						title: "已交卷！",
						icon: "none"
					})
				}else{
					uni.$u.http.post('/Examination/zhuangtao', itme).then(res => {
						let td=res.data;
						if(td==0){
							uni.redirectTo({
								url: "../examination/index?id=" + itme.id
							})
						}else if(td==1){
							uni.showToast({
								title: "未到考试时间！",
								icon: "none"
							})
						}else if(td==2){
							uni.showToast({
								title: "考试时间已截至！",
								icon: "none"
							})
						}
						console.log(res)
					
					}).catch(err => {})
				}
				
				// if(itme.states==2){
				// 	uni.showToast({
				// 		title: "已交卷！",
				// 		icon: "none"
				// 	})
				// }else{
				// 	let date = new Date(itme.stime)
				// 	var time= date.getTime();
				// 	let dates = new Date(itme.etime)
				// 	var times= dates.getTime();
				// 	var timem = Date.now();
				// 	if(time>timem){
				// 		uni.showToast({
				// 			title: "未到考试时间！",
				// 			icon: "none"
				// 		})
				// 	}else{
				// 		if(times>timem){
				// 			uni.redirectTo({
				// 				url: "../examination/index?id=" + itme.id
				// 			})
				// 		}else{
				// 			uni.showToast({
				// 				title: "考试时间已截至！",
				// 				icon: "none"
				// 			})
				// 		}
				// 	}
				// }
				
				
			},
			submit(){
				let xcxUserData = uni.getStorageSync('xcxUserData');
				if (xcxUserData) {
					this.form.uid=xcxUserData.id;
					uni.$u.http.post('/Examination/userzu',this.form).then(res => {
						if(res.code==200){
							uni.redirectTo({
								url: "../examination/index?id=" + res.data
							})
						}else{
							uni.showToast({
								title: '系统维护中',
								icon:'none',
								duration: 1000
							});
						}
						console.log(res)
					
					}).catch(err => {})
				}else{
					uni.showToast({
						title: '您还未登录',
						icon:'none',
						duration: 1000
					});
				}
			}
		}
	}
</script>

<style lang="scss">
	page {
		background-color: #f2f3f5;
	}

	.hbody {
		height: 100rpx;
		background-color: #3d9bf6;
	}

	.xhuan {
		color: #3d9bf6 !important;
	}

	.grid-text {
		font-size: 28rpx;
		color: #666;
		padding: 10rpx 0 20rpx 0rpx;
		/* #ifndef APP-PLUS */
		box-sizing: border-box;
		/* #endif */
	}

	.bbody {
		background: #FFF;
		margin-top: -100rpx;
		padding: 20rpx;
		margin-left: 20rpx;
		margin-right: 20rpx;
		box-shadow: 0px 0px 28rpx 1rpx rgba(78, 101, 153, 0.14);
		border-radius: 15rpx;
	}

	.cbody {
		// background: #FFF;
		// padding: 20rpx;
		// margin:10rpx;
		// box-shadow: 0px 0px 28rpx 1rpx rgba(78, 101, 153, 0.14);
		// border-radius: 15rpx;
		margin-bottom: 30rpx;

		.com-item {
			padding-left: 20rpx;
			padding-right: 20rpx;
			margin-top: 20rpx;

			.com-wrap {
				border-radius: 25rpx;
				overflow: hidden;
			}
		}

		
	}

	.tutlei {
		background-color: #ffffff87;
		padding: 10rpx 20rpx;
		border-top-left-radius: 30rpx;
		border-bottom-left-radius: 30rpx;
		float: right;
		margin-top: 10rpx;
		color: #333;
	}

	.bejing {
		background-color: #ffffff87;
		text-align: center;
		border-radius: 50%;
		width: 100rpx;
		height: 100rpx;
		margin-left: 20rpx;
		padding: 40rpx;

	}

	.shuzhif {
		line-height: 40rpx;
		font-size: 36rpx;
	}

	.bicd {
		background: #FFF;
		margin-top: 60rpx;
		padding: 20rpx;
		// margin-left:20rpx;
		// margin-right: 20rpx;
		box-shadow: 0px 0px 28rpx 1rpx rgba(78, 101, 153, 0.14);
		border-radius: 15rpx;
	}

	.button {
		margin: 40rpx 100rpx;
		background-color: #3d9bf6;
		text-align: center;
		padding: 20rpx;
		color: #fff;
		font-size: 30rpx;
		background-size: 25rpx;

	}
	.cell{
		background-color: #fff;
		padding: 11rpx;
		font-size: 28rpx;
		margin-bottom: 10rpx;
		border-radius: 15rpx;
		line-height: 45rpx;
	}
	.cell-text{
		color: #484545;
	}
	.cell-text-time{
		color: #484545;
		font-size: 26rpx;
	}
	.cell-text-st{
		color: #2196F3;
		
	}
	.u-form-item__body__left__content__label {
	    color: #666 !important;
	    font-size: 28rpx !important;
	}
	
</style>
