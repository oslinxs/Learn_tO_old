<template>
	<view class="u-content">
			<u-parse :content="njosm.contents"></u-parse>
		</view>
</template>

<script>
export default {

	data() {
		return {
			id:2,
			njosm:null
		}
	},
	onLoad(e) {
		uni.hideShareMenu({
		  menus: ['shareAppMessage', 'shareTimeline']
		})
		this.id=e.id;
		this.getData(this.id)
	},
	methods:{
		getData(id) {
			uni.$u.http.post('/Operate/byData', {
					"id": id
				})
				.then(res => {
					console.log(res)
					this.njosm=res.data
					uni.setNavigationBarTitle({
						title: res.data.title
					});
		
				})
		},
	}
}	
</script>


<style lang="scss" scoped>
    .u-content {
        padding: 24rpx;
    }
</style>