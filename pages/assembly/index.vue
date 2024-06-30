<template>
	<view class="body">
		<view  class="tiaojian">选择您要测试条件：</view>
		<u-cell-group :border="true">
			<u-cell :border="true" title="单选题">
				<u-number-box slot="right-icon" v-model="form.dax" :min="0" :max="100" >
				</u-number-box>
			</u-cell>
			<u-cell :border="true" title="多选题">
				<u-number-box slot="right-icon" v-model="form.dx" :min="0" :max="100">
				</u-number-box>
			</u-cell>
			<u-cell :border="true" title="判断题">
				<u-number-box slot="right-icon" v-model="form.pd" :min="0" :max="100">
				</u-number-box>
			</u-cell>
			<u-cell :border="true" title="填空题">
				<u-number-box slot="right-icon" v-model="form.tk" :min="0" :max="100">
				</u-number-box>
			</u-cell>
			<u-cell :border="true" title="简答题">
				<u-number-box slot="right-icon" v-model="form.jd" :min="0" :max="100">
				</u-number-box>
			</u-cell>
		</u-cell-group>
		<view @click="submit" class="button">马上测试</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				value1: 0,
				form: {
					paperid:0,
					uid:0,
					dax:0,
					dx:0,
					pd:0,
					tk:0,
					jd:0
				}

			};
		},
		onLoad() {
			uni.hideShareMenu({
			  menus: ['shareAppMessage', 'shareTimeline']
			})
		},
		onShow() {
			let shujuanid=uni.getStorageSync('xz_item');
			if(shujuanid){
				this.form.paperid=shujuanid;
			}
			let uid=uni.getStorageSync('uid');
			if(uid){
				this.form.uid=uid;
			}
		},
		methods: {
			submit() {
				console.log(this.form)
				uni.$u.http.post('/PaperTitleSubjectUser/zadd',this.form).then(res => {
					uni.navigateTo({
						url:"../examination/index?id="+res.data+"&lx=0"
					})
					
				}).catch(err => {})
			}
		},
	};
</script>
<style lang="scss">
	.body {
		padding: 20rpx;
	}
	.tiaojian{
		padding: 20rpx;
		color: #3e3c3cdd;
	}
	.button{
		margin: 40rpx 100rpx;
		background-color: #3d9bf6;
		text-align: center;
		padding: 20rpx;
		color: #fff;
		font-size: 30rpx;
		background-size: 25rpx;
		
	}
	
</style>
