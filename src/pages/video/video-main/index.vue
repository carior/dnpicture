<template>
	<scroll-view scroll-y enable-flex class="video_main" @scrolltolower="handleScrolltolower">
		<view class="video_item"
		v-for="item in videowp"
		:key="item.id"
		@click="handleGoVideo(item)"
		>
			<image mode="widthFix" :src="item.img"></image>
		</view>
	</scroll-view>
</template>

<script>
	export default {
		data() {
			return {
				videowp: [],
				hasMore: true
			}
		},
		props:{
			urlobj: Object
		},
		watch:{
			urlobj() {
				// 点击分段器 清空旧的数组
				this.videowp = []
				
				this.getList()
			}
		},
		mounted() {
			// 当组件第一次挂载的时候
			this.getList()
		},
		methods:{
			getList() {
				this.request({
					url: this.urlobj.url,
					data: this.urlobj.params
				})
				.then(res => {
					if(res.res.videowp.length === 0) {
						this.hasMore = false
						uni.showToast({
							title:"没有更多数据了",
							icon:"none"
						})
						return
					}
					this.videowp = [...this.videowp, ...res.res.videowp]
				})
			},
			// 分页
			handleScrolltolower() {
				if(!hasMore) {
					uni.showToast({
						title:"没有更多数据了",
						icon:"none"
					})
					return
				}
				this.urlobj.params.skip += this.urlobj.params.limit
				this.getList()
			},
			// 图片点击事件
			handleGoVideo(item) {
				// 1.将数据存储到全局共享中
				getApp().globalData.video = item
				// 2.页面跳转
				uni.navigateTo({
					url: "/pages/videoPlay/index"
				})
			}
		}
	}
</script>

<style lang="scss" scoped>
	.video_main {
		display: flex;
		flex-wrap: wrap;
		height: calc(100vh - 36px);
		.video_item {
			width: 33.33%;
			border: 5rpx solid #fff;
			images {}
		}
	}
</style>
