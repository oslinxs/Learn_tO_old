<template>
	<view class="questions">
		<view class="test-header">
			<view @click="showModal" v-if="states==1">
				<view>
					<image src="../../static/image/jiaojuan.png"></image>
				</view>
				<text>交卷</text>
			</view>
			<view v-if="states==1">
				<text>{{codeMsg}}</text>
			</view>
			<view v-if="states==2">
				<view>
					<image src="../../static/image/jiaojuan.png"></image>
				</view>
				<text>已交卷</text>
			</view>
			<view @click="showFixed">题标 {{swiperIndex}}/{{kaoshilist.length}}</view>
		</view>
		<swiper :current="swiperIndex - 1" @change="swiperChange" class="questions-cont">
			<swiper-item v-for="(item,index) in kaoshilist" :key="index" class="swiper-item">
				<view class="test-main">
					<view class="test-title">
						<text v-if="item.lx==0">单选题</text>
						<text v-if="item.lx==1">多选题</text>
						<text v-if="item.lx==2">判断题</text>
						<view>{{item.title}}</view>
					</view>
					<view class="test-cont" v-if="item.lx<3 && istest==0 ">
						<view @click="testItem(item,index,'A')" v-if="item.lx<3 && item.a!=''"
							:class="ifzhaung(item.uda,'A') ? 'active_true':''">
							<view>A</view>
							<text>{{item.a}}</text>
						</view>
						<view @click="testItem(item,index,'B')" v-if="item.lx<3 && item.b!=''"
							:class="ifzhaung(item.uda,'B') ? 'active_true':''">
							<view>B</view>
							<text>{{item.b}}</text>
						</view>
						<view @click="testItem(item,index,'C')" v-if="item.lx<2 && item.c!=''"
							:class="ifzhaung(item.uda,'C') ? 'active_true':''">
							<view>C</view>
							<text>{{item.c}}</text>
						</view>
						<view @click="testItem(item,index,'D')" v-if="item.lx<2 && item.d!=''"
							:class="ifzhaung(item.uda,'D') ? 'active_true':''">
							<view>D</view>
							<text>{{item.d}}</text>
						</view>
						<view @click="testItem(item,index,'E')" v-if="item.lx==1 && item.aname!=undefined"
							:class="ifzhaung(item.uda,'E') ? 'active_true':''">
							<view>E</view>
							<text>{{item.aname}}</text>
						</view>
						<view @click="testItem(item,index,'F')" v-if="item.lx==1 && item.bname!=undefined"
							:class="ifzhaung(item.uda,'F') ? 'active_true':''">
							<view>F</view>
							<text>{{item.bname}}</text>
						</view>
					</view>
					<view class="test-cont" v-if="item.lx<3 && istest==1 ">
						<view @click="testItem(item,index,'A')" v-if="item.lx<3 && item.a!=''"
							:class="ifzhaung(item.uda,'A') ? 'active_true':''">
							<view>A</view>
							<text>{{item.a}}</text>
						</view>
						<view v-if="item.lx<3 && item.b!=''" :class="ifzhaung(item.uda,'B') ? 'active_true':''">
							<view>B</view>
							<text>{{item.b}}</text>
						</view>
						<view v-if="item.lx<2 && item.c!=''" :class="ifzhaung(item.uda,'C') ? 'active_true':''">
							<view>C</view>
							<text>{{item.c}}</text>
						</view>
						<view v-if="item.lx<2 && item.d!=''" :class="ifzhaung(item.uda,'D') ? 'active_true':''">
							<view>D</view>
							<text>{{item.d}}</text>
						</view>
						<view v-if="item.lx==1 && item.aname!=undefined"
							:class="ifzhaung(item.uda,'E') ? 'active_true':''">
							<view>E</view>
							<text>{{item.aname}}</text>
						</view>
						<view @ v-if="item.lx==1 && item.bname!=undefined"
							:class="ifzhaung(item.uda,'F') ? 'active_true':''">
							<view>F</view>
							<text>{{item.bname}}</text>
						</view>
					</view>
				</view>
				<!-- <view class="test-describe" v-if="states==2">
					<view class="describe-cont">
						<text>正确答案：<text>{{item.da}}</text></text>
						<text>您选择的是：<text>{{item.uda}}</text></text>
						<text>解析：{{item.analysis}}</text>
					</view>
				</view> -->
				<view class="test-describe" v-if="states==2&&item.uda!=''">
					<view class="describe-cont">
						<text>正确答案：<text>{{item.da}}</text></text>
						<text>您选择的是：<text>{{item.uda}}</text></text>
						<text>解析：{{item.analysis}}</text>
					</view>
				</view>
				<view class="test-describe" v-if="bydata.lx==0&&states==1&&item.uda">
					<view class="describe-cont">
						<text>正确答案：<text>{{item.da}}</text></text>
						<text>您选择的是：<text>{{item.uda}}</text></text>
						<text>解析：{{item.analysis}}</text>
					</view>
				</view>
			</swiper-item>
		</swiper>
		<view class="fixed-bottom" :class="{active:show_hide}" @click.stop="hideFixed">
			<view class="tibiao" @click.stop>
				<view class="tibiao-bar">
					<text class="tibiao-no3">未答</text>
					<view class="tibiao-null"></view>
					<text class="tibiao-yes2">已答</text>
					<view class="tibiao-yes"></view>
				</view>
				<view>
					<text v-for="(item,index) in kaoshilist.length" :key="index"
						:class="{active_o:kaoshilist[index].uda}" @click="topic(index)">{{item+1}}</text>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				kaoshilist: [],
				bydata: {},
				id: 0,
				states: 0,
				swiperIndex: 1, //swiper 当前下标
				show_hide: false, //题标是否隐藏 默认false 为隐藏
				istest: 0, //1为练习 2为模拟 3为考试
				timer: null,
				ctime: 0,
				codeMsg: "",
				zhaung: 0
			}
		},
		onLoad(e) {
			uni.removeStorageSync('ksitme');
			uni.hideShareMenu({
				menus: ['shareAppMessage', 'shareTimeline']
			})
			this.id = e.id;
			this.getkaoshi(e.id);
			
			
		},
		onShow() {
			this.getkaoshi(this.id);
		},
		methods: {
			getkaoshi(id) {
				uni.$u.http.post('/Examination/bydata', {
					id: id
				}).then(res => {
					console.log(res)
					uni.setNavigationBarTitle({
						title: res.data.examinationUser.title
					});
					this.states = res.data.states
					this.bydata = res.data.examinationUser
					this.kaoshilist = res.data.array;
					if (res.data.examinationUser.lx == 2) {
						if (res.data.states == 1) {
							// let dates = new Date(this.bydata.etime)
							// var times= dates.getTime();
							// var timem = Date.now();
							// let datsxc=Math.floor((times-timem)/1000)
							// console.log(datsxc)
							
							
							// let dtime = (this.bydata.ctime - this.bydata.cid) * 60;
							this.ctime =  res.data.time;
							uni.setStorageSync('ksitme', 1)
							this.gettime();
						}
					} else if (res.data.examinationUser.lx == 1) {
						if (res.data.states == 1) {
							let dtime = (60 - this.bydata.cid) * 60;
							this.ctime = dtime;
							uni.setStorageSync('ksitme', 1)
							this.gettime();
						}
					}
					 else {
						if (res.data.states == 1) {
							this.ctime = 60;
							uni.setStorageSync('ksitme', 1)
							this.gettimes();
						}
					}

				}).catch(err => {})
			},


			showModal() { // 左上角交卷
				let than = this;


				let list = this.kaoshilist;
				
				console.log(bydata)
				
				if(this.bydata.lx==0){
					uni.showModal({
						title: '提示',
						content: '确认要交卷吗？',
						success: function(res) {
							if (res.confirm) {
								than.distinctI(2);
							} else if (res.cancel) {
								console.log('用户点击取消');
							}
						}
					});
				}else{
					let a = 0;
					let b = 0;
					for (var item of list) {
						a = a + 1;
						if (item.uda == undefined) {
							b = b + 1;
							uni.showToast({
								title: "第" + a + "题未答，不可交卷",
								icon: "none"
							})
							break;
						}
					
					}
					if (b == 0) {
					
						uni.showModal({
							title: '提示',
							content: '确认要交卷吗？',
							success: function(res) {
								if (res.confirm) {
									than.distinctI(2);
								} else if (res.cancel) {
									console.log('用户点击取消');
								}
							}
						});
					}
					
				}
				




			},
			swiperChange(e) { //swiper滑动
				this.swiperIndex = e.detail.current + 1; //当前题
			},
			testItem(a, b, c) {
				if (this.states == 1) {


					let that = this
					// console.log(a,b,c)
					let lx = a.lx;
					let list = this.kaoshilist;
					if (lx == 1) {
						if (list[b].uda == undefined || list[b].uda == '') {
							list[b].uda = c
						} else {

							let uda = list[b].uda

							console.log(uda)
							let data = "";
							let st = this.ifzhaung(uda, c);
							if (st) {
								let arr = uda.split(",");
								for (let j = 0; j < arr.length; j++) {

									if (arr[j] != c) {
										if (data == "") {
											data = arr[j];
										} else {
											data = data + "," + arr[j];
										}

									}

								}
							} else {
								data = uda + "," + c
							}

							let sta = this.ifzhaung(data, "A");
							let stb = this.ifzhaung(data, "B");
							let stc = this.ifzhaung(data, "C");
							let std = this.ifzhaung(data, "D");
							let ste = this.ifzhaung(data, "E");
							let stf = this.ifzhaung(data, "F");
							let bydate = "";
							if (sta) {
								bydate = "A";
							}
							if (stb) {
								if (bydate != "") {
									bydate = bydate + ",B";
								} else {
									bydate = "B";
								}
							}
							if (stc) {
								if (bydate != "") {
									bydate = bydate + ",C";
								} else {
									bydate = "C";
								}
							}
							if (std) {
								if (bydate != "") {
									bydate = bydate + ",D";
								} else {
									bydate = "D";
								}
							}
							if (ste) {
								if (bydate != "") {
									bydate = bydate + ",E";
								} else {
									bydate = "E";
								}
							}
							if (stf) {
								if (bydate != "") {
									bydate = bydate + ",F";
								} else {
									bydate = "F";
								}
							}

							// console.log("data",bydate)
							list[b].uda = bydate

						}
					} else {
						list[b].uda = c


					}
					this.kaoshilist = list;
					this.$forceUpdate();
				}
			},

			gettime() {

				if (!this.timer) {
					
					this.timer = setInterval(() => {
						if (this.ctime > 0) {
							this.ctime--;
							const ksitme = uni.getStorageSync('ksitme');
							if(ksitme){
								
								// console.log(this.ctime)
								if (this.ctime !== 0) {
									uni.setStorageSync('ksitme', 1)
									this.zhaung++;
									var dtime = this.ctime
									let h = parseInt(dtime / 60 / 60 % 24)
									h = h < 10 ? '0' + h : h
									let m = parseInt(dtime / 60 % 60)
									m = m < 10 ? '0' + m : m
									let s = parseInt(dtime % 60)
									s = s < 10 ? '0' + s : s
									this.codeMsg = "考试倒计时：" + (h * 60 + m) + ":" + s + " S";
									if (this.zhaung > 59) {
										this.distinctI(1);
										this.zhaung = 0;
									}

								} else {
									uni.removeStorageSync('ksitme');
									clearInterval(this.timer);
									this.codeMsg = "";
									this.ctime = 0;
									this.timer = null;
									this.distinctI(2);
								}
							}
						}
					}, 1000)
				}
			},
			gettimes() {

				if (!this.timer) {
					this.timer = setInterval(() => {
						if (this.ctime > 0) {
							this.ctime--;
							if (this.ctime == 0) {
								// clearInterval(this.timer);
								this.codeMsg = "";
								this.ctime = 60;
								this.timer = null;
								this.distinctI(1);
								console.log(1)
							}
						}
					}, 1000)
				}
			},
			distinctI(states) {
				uni.$u.http.post('/Examination/da', {
					array: this.kaoshilist,
					id: this.id,
					states: states
				}).then(res => {
					console.log(res)
					if (states == 2) {
						uni.removeStorageSync('ksitme');
						clearInterval(this.timer);
						uni.showToast({
							title: "交卷成功！"
						})
						setTimeout(function() {
							uni.switchTab({
								url: '../juan/index'
							})
						}, 800);
					}


				}).catch(err => {})
			},
			ifzhaung(sx, c) {
				if (sx != null) {
					let arr = sx.split(",");

					for (let j = 0; j < arr.length; j++) {
						if (arr[j] == c) {
							return true;
							break;
						}
						// console.log(arr[j])
					}
				}

				return false;
			},

			topic(e) {
				this.swiperIndex = e + 1
				this.show_hide = !this.show_hide
			},
			showFixed() {
				this.show_hide = !this.show_hide
			},
			hideFixed() {
				this.show_hide = !this.show_hide
			},
			nullFixed() {
				//占位 不可删
			}
		}
	}
</script>

<style lang="less">
	page {
		height: 100%;
		background: #f5f5f5;
	}

	.questions {
		height: 100%;
		position: relative;

		.test-header {
			position: absolute;
			top: 0;
			left: 0;
			z-index: 10;
			width: 690rpx;
			padding: 0 30rpx;
			display: flex;
			justify-content: space-between;
			height: 34px;
			line-height: 34px;
			border-bottom: solid 1px #ddd;
			background: #f5f5f5;

			view {
				// color: #666;
				color: #3d9bf6;
				font-size: 34rpx;
				display: flex;

				image {
					margin-top: 10px;
					margin-right: 5px;
					width: 14px;
					height: 14px;
				}

				&:nth-child(1) {
					font-size: 30rpx;

					text {
						color: #3d9bf6;
						font-weight: 600;
					}
				}

				&:nth-child(3) {
					font-size: 30rpx;

					text {
						color: #666;
					}
				}
			}
		}

		.questions-cont {
			height: 100%;
			display: flex;
			flex-wrap: nowrap;
			transition: all .5s;

			.swiper-item {
				width: 100vw;
				min-width: 100vw;
				max-width: 100vw;
				height: 100%;
				overflow: auto;

				.test-main {
					padding: 0 20rpx;
					margin: 49px 30rpx;
					margin-bottom: 20px;
					background: #fff;

					.test-title {
						font-size: 28rpx;
						color: #666;
						padding: 20rpx 0;

						text {
							font-size: 20rpx;
							color: #fff;
							background: #2196F3;
							padding: 2px 5px;
							margin-right: 5px;
							border-radius: 3px;
						}

					}

					.test-cont {
						display: flex;
						flex-direction: column;
						padding-bottom: 20rpx;

						&>view {
							min-height: 40px;
							line-height: 40px;
							display: flex;

							view {
								width: 30px;
								height: 30px;
								line-height: 30px;
								margin-top: 4px;
								margin-right: 7px;
								font-size: 14px;
								border: solid 1px #ddd;
								border-radius: 50%;
								box-shadow: 2px 2px 5px #e5e5e5;
								text-align: center;
								color: #666;
							}

							image {
								max-width: 560rpx;
								max-height: 280rpx;
								margin-bottom: 20rpx;
							}

							text {
								font-size: 28rpx;
								color: #666;
							}

							&.active_true {
								view {
									background: #1a7bb9;
									border: solid 1px #1a7bb9;
									color: #fff;
									font-size: 15px;
								}

								text {
									color: #1a7bb9;
								}
							}

							&.active {
								view {
									background: #FF6667;
									border: solid 1px #FF6667;
									color: #fff;
									font-size: 15px;
								}

								text {
									color: #FF6667;
								}
							}
						}
					}
				}

				.test-btn {
					background: #1FB19E;
					font-size: 28rpx;
					border-radius: 6px;
					text-align: center;
					line-height: 40px;
					color: #fff;
					height: 40px;
					width: 150px;
					margin: 20px auto;
				}

				.test-describe {
					padding: 0 20rpx;
					margin-bottom: 20px;
					border-top: solid 1px #ccc;

					.describe-title {
						height: 48px;
						line-height: 48px;
						display: flex;

						text {
							color: #666;
							font-size: 28rpx;
						}

						image {
							width: 14px;
							height: 14px;
							margin-top: 17px;
							margin-left: 3px;
						}
					}

					.describe-cont {
						background: #f5f5f5;
						padding: 12rpx;
						display: flex;
						flex-direction: column;

						&>text {
							color: #666;
							font-size: 28rpx;
							line-height: 24px;

							&:nth-child(3) {
								font-size: 28rpx;
								line-height: 20px;
							}
						}
					}
				}
			}
		}

		.fixed-bottom {
			width: 100%;
			height: 100%;
			position: fixed;
			left: 0;
			top: 0;
			z-index: 999;
			background: rgba(0, 0, 0, .4);
			pointer-events: none;
			opacity: 0;
			transition: all .3s;

			.tibiao {
				position: absolute;
				bottom: 0;
				left: 0;
				background: #fff;
				width: 710rpx;
				overflow: auto;
				height: 50%;
				padding: 30rpx 20rpx;

				.tibiao-bar {
					display: flex;
					flex-direction: row-reverse;
					padding-bottom: 30rpx;
					border-bottom: solid 1px #ddd;
					height: 16px;
					line-height: 16px;

					view {
						width: 10px;
						height: 10px;
						border-radius: 50%;
						margin-left: 30rpx;
						margin-top: 3px;

						&.tibiao-yes {
							background: #1a7bb9;
						}

						&.tibiao-no {
							background: #FF6667;
						}

						&.tibiao-null {
							background: #999;
						}
					}

					text {
						margin-left: 3px;
						display: inline-block;
						font-size: 13px;

						&.tibiao-yes2 {
							color: #1a7bb9;
						}

						&.tibiao-no2 {
							color: #FF6667;
						}

						&.tibiao-no3 {
							color: #666;
						}
					}
				}

				&>view {
					&:nth-child(2) {
						text {
							margin-left: 22rpx;
							margin-top: 10rpx;
							display: inline-block;
							width: 100rpx;
							height: 100rpx;
							line-height: 100rpx;
							text-align: center;
							border-radius: 50%;
							font-size: 14px;
							color: #666;
							background: #e4e4e4;

							&:nth-child(6n+1) {
								margin-left: 0;
							}

							&.active_o {
								background: #beddf1;
								color: #1a7bb9;
							}

							&.active_t {
								background: #fecece;
								color: #e3494a;
							}
						}
					}
				}
			}

			&.active {
				opacity: 1;
				pointer-events: auto;
				transition: all .3s;
			}
		}
	}

	.datacd {
		padding-bottom: 20rpx;
	}
</style>
