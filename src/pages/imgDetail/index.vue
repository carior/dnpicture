<template>
	<view>
		<!-- 用户信息 开始 -->
		<view class="user_info">
			<view class="user_icon">
				<image src="http://img0.adesk.com/download/5d36a43ac8cfb750c51bb670" mode="widthFix"></image>
			</view>
			<view class="user_desc">
				<view class="user_name">蛋蛋君</view>
				<view class="user_time">{{imgDetail.cnTime}}</view>
			</view>
		</view>
		<!-- 用户信息 结束 -->
		
		<!-- 高清大图 开始 -->
		<view class="high_img">
			<swiper-action @swiperAction="handleSwiperAction">
				<image mode="aspectFills" :src="imgDetail.thumb"></image>
			</swiper-action>
		</view>
		<!-- 高清大图 结束 -->
		
		<!-- 点赞 开始 -->
		<view class="user_rank">
			<view class="rank">
				<text class="iconfont icondianzan">{{imgDetail.rank}}</text>
			</view>
			<view class="user_collect">
				<text class="iconfont iconshoucang">收藏</text>
			</view>
		</view>
		<!-- 点赞 结束 -->
		
		<!-- 专辑 开始 -->
		<view class="album_wrap" v-if="album.length">
			<view class="album_title">相关</view>
			<view class="album_list">
				<view class="album_item"
				v-for="item in album"
				:key="item.id"
				>
					<view class="album_cover">
						<image :src="item.cover" mode="aspectFill"></image>
					</view>
					<!-- 右边 -->
					<view class="album_info">
						<view class="album_info_text">专辑</view>
						<view class="album_name">{{item.name}}</view>
						<text class="iconfont iconiconfontjiantou4"></text>
					</view>
				</view>
			</view>
		</view>
		<!-- 专辑 结束 -->
		
		<!-- 最热评论 开始 -->
		<view class="comment hot" v-if="hot.length">
			<view class="comment_title">
				<text class="iconfont iconhot1"></text>
				<text class="comment_text">最热评论</text>
			</view>
			<view class="comment_list">
				<view class="comment_item"
				v-for="item in hot"
				:key="item.id"
				>
					<!-- 用户信息 -->
					<view class="comment_user">
						<view class="user_icon">
							<image mode="widthFix" :src="item.user.avatar"></image>
							<view class="user_name">
								<view class="user_nickname">{{item.user.name}}</view>
								<view class="user_time">{{item.atime}}</view>
							</view>
							<view class="user_badge">
								<image
								v-for="badge in item.user.title"
								:key="badge.icon"
								:src="badge.icon"
								></image>
							</view>
						</view>
					</view>
					<!-- 评论数据 -->
					<view class="comment_desc">
						<view class="comment_content">{{item.content}}</view>
						<view class="comment_like">
							<text class="iconfont icondianzan">{{item.size}}</text>
						</view>
					</view>
				</view>
			</view>
		</view>
		<!-- 最热评论 结束 -->
		
		<!-- 最新评论 开始 -->
		<view class="comment new" v-if="comment.length">
			<view class="comment_title">
				<text class="iconfont iconpinglun"></text>
				<text class="comment_text">最新评论</text>
			</view>
			<view class="comment_list">
				<view class="comment_item"
				v-for="item in comment"
				:key="item.id"
				>
					<!-- 用户信息 -->
					<view class="comment_user">
						<view class="user_icon">
							<image mode="widthFix" :src="item.user.avatar"></image>
							<view class="user_name">
								<view class="user_nickname">{{item.user.name}}</view>
								<view class="user_time">{{item.cnTime}}</view>
							</view>
							<view class="user_badge">
								<image
								v-for="badge in item.user.title"
								:key="badge.icon"
								:src="badge.icon"
								></image>
							</view>
						</view>
					</view>
					<!-- 评论数据 -->
					<view class="comment_desc">
						<view class="comment_content">{{item.content}}</view>
						<view class="comment_like">
							<text class="iconfont icondianzan">{{item.size}}</text>
						</view>
					</view>
				</view>
			</view>
		</view>
		<!-- 最新评论 结束 -->
		
		<!-- 下载 开始 -->
		<view class="download">
			<view class="download_btn" @click="handleDownload">
				下载图片
			</view>
		</view>
		<!-- 下载 结束 -->
	</view>
</template>

<script>
	import moment from "moment"
	import swiperAction from '@/components/swiperAction.vue'
	// 设置语言为中文
	moment.locale('zh-cn')
	export default {
		components:{
			swiperAction
		},
		data() {
			return {
				// 图片信息对象
				imgDetail: {},
				// 专辑数据
				album: [],
				// 最热评论
				hot: [],
				// 最新评论
				comment: [],
				// 图片的索引
				imgIndex: 0
			}
		},
		onLoad() {
			const {imgIndex} = getApp().globalData
			this.imgIndex = imgIndex
			this.getDate()
		},
		methods:{
			// 给当前页面赋值
			getDate(){
				const {imgList} = getApp().globalData
				this.imgDetail = imgList[this.imgIndex]
				// xxx年前
				this.imgDetail.cnTime = moment(this.imgDetail.atime * 1000).fromNow()
				// 获取图片详情的id
				this.getComments(this.imgDetail.id)
			},
			getComments(id) {
				this.request({
					url: `https://service.picasso.adesk.com/v2/wallpaper/wallpaper/${id}/comment`
				})
				.then(res => {
					this.album = res.res.album
					
					res.res.hot.forEach(v => v.cnTime = moment(v.atime*1000).fromNow())
					res.res.comment.forEach(v => v.cnTime = moment(v.atime*1000).fromNow())
					
					this.hot = res.res.hot
					this.comment = res.res.comment
				})
			},
			// 滑动事件
			handleSwiperAction(e) {
				const {imgList} = getApp().globalData
				const {direction} = e
				if(direction === 'left' && this.imgIndex >= 0) {
					if (this.imgIndex == imgList.length - 1) {
						uni.showToast({
							title: "没有数据了",
							icon: "none"
						})
						return
					}
					this.imgIndex++ 
					this.getDate()
				} else if(direction=== 'right' && this.imgIndex <= imgList.length - 1){
					this.imgIndex--
					this.getDate()
				} else {
					uni.showToast({
						title: "没有数据了",
						icon: "none"
					})
				}
			},
			// 点击下载图片 async await promise
			async handleDownload () {
				await uni.showLoading({
					title:"下载中"
				})
				// 1. 将远程文件下载到小程序的内存中 tempFilePath
				const result = await uni.downloadFile({
					url: this.imgDetail.img
				})
				const {tempFilePath} = result[1]
				// 2. 将小程序内存中的临时文件 下载到本地上
				const result2 = await uni.saveImageToPhotosAlbum({
					filePath: tempFilePath
				})
				// 3.提示用户下载成功
				uni.hideLoading()
				await uni.showToast({
					title: "下载成功"
				})
			}
		}
	}
</script>

<style lang="scss" scoped>
	.user_info {
		display: flex;
		padding: 20rpx;
		.user_icon {
			padding: 0 20rpx;
			image {
				width: 88rpx;
				border-radius: 50%;
			}
		}
		.user_desc {
			.user_name {
				color: #000;
				font-weight: 600;
			}
			.user_time {
				color: #ccc;
				font-size: 24rpx;
				padding: 10rpx 0;
			}
		}
	}
	.high_img {
		image {
			height: 300rpx;
		}
	}
	
	.user_rank {
		display: flex;
		height: 80rpx;
		line-height: 80rpx;
		border-bottom: 5rpx solid #eee;
		.rank {
			flex: 1;
			display: flex;
			justify-content: center;
		}
		.user_collect {
			flex: 1;
			display: flex;
			justify-content: center;
		}
	}
	
	.album_wrap {
		padding: 20rpx;
		.album_title {
			padding: 10rpx 0;
		}
		.album_list {
			padding: 10rpx 0;
			.album_item {
				display: flex;
				border-bottom: 10rpx solid #eee;
				.album_cover {
					flex: 1;
					image {
						width: 180rpx;
						height: 180rpx;
					}
				}
				.album_info {
					flex: 3;
					padding-left: 20rpx;
					position: relative;
					.album_info_text {
						width: 100rpx;
						height: 50rpx;
						background-color: $color;
						color: #fff;
						display: flex;
						justify-content: center;
						align-items: center;
					}
					.album_name {
						padding: 10rpx 0;
						color: #888;
					}
					.iconfont {
						font-size: 40rpx;
						position: absolute;
						top: 50%;
						transform: translateY(-50%);
						right: 10%;
						color: #000;
					}
				}
			}
		}
	}

	.comment {
		.comment_title {
			padding: 15rpx;
			.iconfont {
				color: red;
				font-size: 40rpx;
			}
			.comment_text {
				font-weight: 600;
				font-size: 28rpx;
				color: #666;
				margin-left: 10rpx;
			}
		}
		.comment_list {
			.comment_item {
				border-bottom: 15rpx solid #eee;
				.comment_user {
					display: flex;
					padding: 20rpx 0;
					.user_icon {
						display: flex;
						justify-content: center;
						align-items: center;
						width: 100%;
						image {
							width: 90rpx;
							height: 90rpx;
						}
					}
					.user_name {
						flex: 1;
						.user_nickname {
							color: #777;
						}
						.user_time {
							color: #ccc;
							font-size: 24rpx;
							padding: 5rpx;
						}
					}
					.user_badge {
						image {
							width: 40rpx;
							height: 40rpx;
							display: inline-block;
							margin-right: 5rpx;
						}
					}
				}
				.comment_desc {
					display: flex;
					padding: 30rpx;
					.comment_content{
						flex: 1;
						padding-left: 15%;
					}
					.comment_like {
						text-align: right;
						.iconfont {}
					}
				}
			}
		}
	}
	.new {
		.iconpinglun {
			color: aqua!important;
		}
	}
	.download {
		display: flex;
		height: 120rpx;
		align-items: center;
		justify-content: center;
		.download_btn {
			width: 90%;
			height: 80rpx;
			background-color: $color;
			color: #fff;
			font-size: 45rpx;
			font-weight: 600;
			display: flex;
			align-items: center;
			justify-content: center;
		}
	}
	
</style>
