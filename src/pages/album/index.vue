<template>
	<view>
		<!-- 专辑背景 开始 -->
		<view class="album_bg">
			<image mode="widthFix" :src="album.cover"></image>
			<view class="album_info">
				<view class="album_name">{{album.name}}</view>
				<view class="album_btn">关注专辑</view>
			</view>
		</view>
		<!-- 专辑背景 结束 -->
		
		<!-- 专辑作者 开始 -->
		<view class="album_author">
			<view class="alum_author_info">
				<image mode="widthFix" :src="album.user.avatar"></image>
				<view class="author_name">{{album.user.name}}</view>
			</view>
			<view class="alum_author_desc">
				<!-- text 使用 可以换行-->
				<text>{{album.desc}}</text>
			</view>
		</view>
		<!-- 专辑作者 结束 -->
		
		<!-- 列表 开始 -->
		<view class="album_list">
			<view class="album_item"
			v-for="(item, index) in wallpaper"
			:key="item.id"
			>
				<go-detail :list="wallpaper" :index="index">
					<image mode="aspectFill" :src="item.thumb"></image>
				</go-detail>
			</view>
		</view>
		<!-- 列表 结束 -->
	</view>
</template>

<script>
	import goDetail from "@/components/goDetail.vue"
	export default {
		data() {
			return {
				params: {
					limit: 30,
					order: "new",
					skip: 0,
					// 1 返回值中有 album对象 表示第一次请求数据
					// 0 返回值只有 wallpaper 数组 不是第一次请求数据
					first: 1
				},
				wallpaperParams: {
					limit: 30,
					order: "hot",
					skip: 0
				},
				id: -1,
				album: {},
				wallpaper: [],
				hasMore: true
			}
		},
		components:{
			goDetail
		},
		onLoad(options) {
			this.id = options.id
			// this.id = "5965cd0be7bce7312ef79fbf"
			this.getList()
			this.getWallpaper()
		},
		// 页面触底 上拉加载下一页事件 全屏滚动
		onReachBottom() {
			if (this.hasMore) {
				this.params.first = 0
				this.wallpaperParams.skip += this.wallpaperParams.limit
				this.getWallpaper()
			} else {
				uni.showToast({
					title:"没有更多数据了",
					icon: "none"
				})
			}
		},
		methods: {
			getList() {
				this.request({
					url: `https://service.picasso.adesk.com/v1/wallpaper/album/${this.id}/wallpaper`,
					data: this.params
				})
				.then(res => {
					this.album = res.res.album
					// this.wallpaper = res.res.wallpaper
				})
			},
			getWallpaper() {
				this.request({
						url: "https://service.picasso.adesk.com/v3/homepage/vertical",
						data: this.wallpaperParams
					})
					.then(res => {
						if (res.res.vertical.length === 0) {
							this.hasMore = false
							uni.showToast({
								title:"没有更多数据了",
								icon: "none"
							})
							return
						}
						this.wallpaper = [...this.wallpaper, ...res.res.vertical]
					})
			},
		}
	}
</script>

<style lang="scss" scoped>
	.album_bg {
		position: relative;
		image {
			
		}
		.album_info {
			position: absolute;
			width: 100%;
			left: 0;
			bottom: 0;
			display: flex;
			justify-content: space-between;
			height: 80rpx;
			align-items: center;
			color: #fff;
			padding: 0 15rpx;
			.album_name {
				font-size: 40rpx;
			}
			.album_btn {
				background-color: $color;
				width: 152rpx;
				height: 60rpx;
				display: flex;
				justify-content: center;
				align-items: center;
				border-radius: 10rpx;
			}
		}
	}
	.album_author {
		padding: 10rpx;
		.alum_author_info {
			padding: 10rpx 0;
			display: flex;
			image {
				width: 50rpx;
			}
			.author_name {
				color: #000;
				margin-left: 15rpx;
			}
		}
		.alum_author_desc {}
	}
	.album_list {
		display: flex;
		flex-wrap: wrap;
		.album_item {
			width: 33.33%;
			border: 3rpx solid #fff;
			image {
				height: 160rpx;
			}
		}
	}
</style>
