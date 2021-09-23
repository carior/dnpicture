<template>
	<scroll-view @scrolltolower="handleToLower" class="recomend_view" scroll-y v-if="recommends.length > 0">
		<!-- 推荐 开始 -->
		<view class="recommend_wrap">
			<navigator class="recommend_item" v-for="item in recommends" :key="item.id" :url="`/pages/album/index?id=${item.target}`">
				<image mode="widthFix" :src="item.thumb"></image>
			</navigator>
		</view>
		<!-- 推荐 结束 -->

		<!-- 月份 开始 -->
		<view class="months_wrap">
			<view class="month_title">
				<view class="month_title_info">
					<view class="month_info">
						<text>{{months.DD}} / </text>
						{{months.MM}}月
					</view>
					<view class="month_text">{{months.title}}</view>
				</view>
				<view class="month_title_more">更多 ></view>
			</view>
			<view class="month_content">
				<view class="month_item" v-for="(item, index) in months.items" :key="item.id">
					<go-detail :list="months.items" :index="index">
						<image mode="aspectFill" :src="item.thumb + item.rule.replace('$<Height>', 360)"></image>
					</go-detail>
					
				</view>
			</view>
		</view>
		<!-- 月份 结束 -->

		<!-- 热门 开始 -->
		<view class="hots_wrap">
			<view class="hots_title">
				<text>热门</text>
			</view>
			<view class="hots_content">
				<view class="hot_item" v-for="(item, index) in hots" :key="item.id">
					<go-detail :list="hots" :index="index">
						<image mode="widthFix" :src="item.thumb"></image>
					</go-detail>
				</view>
			</view>
		</view>
		<!-- 热门 结束 -->
	</scroll-view>
</template>

<script>
	import moment from 'moment'
	import goDetail from "@/components/goDetail.vue"
	export default {
		data() {
			return {
				// 推荐列表
				recommends: [],
				// 月份
				months: [],
				// 热门
				hots: [],
				// 请求的参数
				params: {
					limit: 30,
					order: "hot",
					skip: 0
				},
				// 是否还有下一页
				hasMore: true
			}
		},
		components:{
			goDetail
		},
		mounted() {
			this.getList()
		},
		methods: {
			// 获取接口数据
			getList() {
				this.request({
						url: "https://service.picasso.adesk.com/v3/homepage/vertical",
						data: this.params
					})
					.then(res => {
						// 判断还有没有下一页数据
						if (res.res.vertical.length === 0) {
							this.hasMore = false
							uni.showToast({
								title:"没有更多数据了",
								icon: "none"
							})
							return
						}
						if (this.recommends.length === 0) {
							// 推荐模块
							this.recommends = res.res.homepage[1].items
							// 月份模块
							this.months = res.res.homepage[2]
							// 将时间戳改为18号/1月 moment.js
							this.months.MM = moment(this.months.stime).format("MM")
							this.months.DD = moment(this.months.stime).format("DD")
						}
						// 获取热门数据的列表
						this.hots = [...this.hots, ...res.res.vertical]
					})
			},
			// 滚动条触底事件
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
	.recomend_view {
		height: calc(100vh - 36px);
	}

	.recommend_wrap {
		display: flex;
		flex-wrap: wrap;

		.recommend_item {
			width: 50%;
			border: 5rpx solid #fff;
		}
	}

	.months_wrap {
		.month_title {
			display: flex;
			justify-content: space-between;
			padding: 20rpx;

			.month_title_info {
				color: $color;
				font-size: 30rpx;
				font-weight: 600;
				display: flex;

				.month_info {
					text {
						font-size: 36rpx;
					}
				}

				.month_text {
					font-size: 34rpx;
					color: #666;
					margin-left: 30rpx;
				}
			}

			.month_title_more {
				font-size: 24rpx;
				color: $color;
			}
		}

		.month_content {
			display: flex;
			flex-wrap: wrap;

			.month_item {
				width: 33.33%;
				border: 5rpx solid #fff;
			}
		}
	}

	.hots_wrap {
		.hots_title {
			padding: 20rpx;

			text {
				border-left: 20rpx solid $color;
				padding-left: 20rpx;
				font-size: 34rpx;
				font-weight: 600;
			}
		}

		.hots_content {
			display: flex;
			flex-wrap: wrap;

			.hot_item {
				width: 33.33%;
				border: 5rpx solid #fff;

				image {}
			}
		}
	}
</style>
