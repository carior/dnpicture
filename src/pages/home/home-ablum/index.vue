<template>
	<scroll-view class="album_scroll_view" scroll-y @scrolltolower="handleToLower">
		<!-- 轮播图 开始 -->
		<view class="album_swiper">
			<!-- 
				1. 自动轮播 autoplay
				2. 指示器 indicator-dots
				3. 衔接轮播 circular
				4. swiper 默认高度 150px
				5. image 默认的宽度 320px => 基本样式中 重置成了 100% 默认高度为 240px
				6. 计算图片的宽度按和高度比例 640*284
				7. 把图片的比例也写进到 swiper 标签样式
			 -->
			<swiper autoplay indicator-dots circular>
				<swiper-item
				v-for="item in banner"
				:key="item.id"
				>
					<image :src="item.thumb"></image>
				</swiper-item>
			</swiper>
		</view>
		<!-- 轮播图 结束 -->
		
		<!-- 列表 开始  -->
		<view class="album_list">
			<navigator class="album_item"
			v-for="item in album"
			:key="item.id"
			:url="`/pages/album/index?id=${item.id}`"
			>
				<view class="album_img">
					<image mode="aspectFill" :src="item.cover"></image>
				</view>
				<view class="album_info">
					<view class="album_name">{{item.name}}</view>
					<view class="album_desc">{{item.desc}}</view>
					<view class="album_btn">
						<view class="album_attention">关注</view>
					</view>
				</view>
			</navigator>
		</view>
		<!-- 列表 结束  -->
	</scroll-view>
</template>

<script>
	export default {
		data() {
			return {
				params: {
					limit: 30,
					order: "new",
					skip: 0
				},
				// 轮播图数组
				banner: [],
				// 列表数组
				album: [],
				// 是否还有分页数据
				hasMore: true
			}
		},
		mounted() {
			// 修改页面的标题
			uni.setNavigationBarTitle({
				title: "专辑"
			})
			this.getList()
		},
		methods:{
			// 获取接口数据
			getList() {
				this.request({
					url: "https://service.picasso.adesk.com/v1/wallpaper/album",
					data: this.params
				})
				.then(res => {
					// 判断还有没有下一页数据
					if (res.res.album.length === 0) {
						this.hasMore = false
						uni.showToast({
							title:"没有更多数据了",
							icon: "none"
						})
						return
					}
					if (this.banner.length === 0) {
						this.banner = res.res.banner
					}
					
					this.album = [...this.album, ...res.res.album]
				})
			},
			// 上拉加载执行分页
			handleToLower() {
				if(!this.hasMore) {
					uni.showToast({
						title: "没有更多数据了",
						icon: "none"
					})
					return
				}
				// 1.修改请求参数 skip += limit
				this.params.skip += this.params.limit
				// 2.重新发送请求 getList()
				// 3. 请求回来成功 hots 数据的叠加
				this.getList()
			}
		}
	}
</script>

<style lang="scss" scoped>
	.album_scroll_view {
		height: calc(100vh - 36px);
	}
	.album_swiper {
		swiper {
			// 宽度 750rpx
			height: calc(750rpx * 284 / 640);
			image {
				height: 100%;
			}
		}
	}
	.album_list {
		padding: 10 rpx;
		.album_item {
			padding: 10rpx 0;
			display: flex;
			border-bottom: 1px solid #ccc;
			.album_img {
				flex: 1;
				padding: 10rpx;
				image {
					width: 200rpx;
					height: 200rpx;
				}
			}
			.album_info {
				flex: 2;
				overflow: hidden;
				padding: 0 10rpx;
				.album_name {
					font-size: 30rpx;
					color: #000;
					padding: 10rpx 0;
				}
				.album_desc {
					padding: 10rpx 0;
					font-size: 24rpx;
					text-overflow: ellipsis;
					overflow: hidden;
					white-space: nowrap;
				}
				.album_btn {
					padding: 10rpx;
					display: flex;
					justify-content: flex-end;
					.album_attention {
						color: $color;
						border: 1rpx solid $color;
						padding: 10rpx;
					}
				}
			}
		}
	}
</style>
