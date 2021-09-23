<template>
	<view class="category_tab">
	  <view class="category_tab_title">
	    <view class="title_inner">
	      <uni-segmented-control 
	        :current="current" 
	        :values="items.map(v=> v.title)" 
	        @clickItem="onClickItem" 
	        styleType="text" 
	        activeColor="#d4237a"
	      ></uni-segmented-control>
	    </view>
	    <view class="iconfont iconsearch"></view>
	  </view>
	    <scroll-view class="category_tab_content" scroll-y enable-flex @scrolltolower="handleScrollTolower">
			<view class="cate_item"
			v-for="item in vertical"
			:key="item.id"
			>
				<image :src="item.thumb" mode="widthFix"></image>
			</view>
	    </scroll-view>
	</view>
</template>

<script>
	import { uniSegmentedControl } from '@dcloudio/uni-ui'
	export default {
		components: {
		  uniSegmentedControl
		},
		data() {
		  return {
		    items: [
		      {title: "最新", order: "new"},
		      {title: "热门", order: "hot"},
		    ],
		    current: 0,
			params: {
				limit: 30,
				skip: 0,
				order: "new"
			},
			id: 0,
			vertical: [],
			hasMore: true,
		  }
		},
		methods: {
		  onClickItem(e) {
		    if(this.current !== e.currentIndex) {
		      this.current = e.currentIndex
		    } else {
				// 点击了相同的标题
				return
			}
			this.params.order = this.items[e.currentIndex].order
			this.params.skip = 0
			this.vertical = []
			this.getList()
		  },
		  getList() {
			  this.request({ 
			  	url: `https://service.picasso.adesk.com/v1/vertical/category/${this.id}/vertical`,
				data: this.params
			  })
			  .then(res => {
				  if(res.res.vertical.length === 0) {
					  this.hasMore = false
					  uni.showToast({
					  	title:"没有更多数据了",
					  	icon:"none"
					  })
					  return
				  }
			  	this.vertical = [...this.vertical, ...res.res.vertical]
			  })
		  },
		  // 加载下一页数据
		  handleScrollTolower() {
			  if (!this.hasMore) {
				  uni.showToast({
				  	title:"没有更多数据了",
					icon:"none"
				  })
				  return
			  }
			  this.params.skip += this.params.limit
			  this.getList()
		  }
		},
		onLoad(options) {
			this.id = options.id
			this.getList()
		},
	}
</script>

<style lang="scss" scoped>
.category_tab {
  .category_tab_title {
    position: relative;
    .title_inner {
      width: 60%;
      margin: 0 auto;
    }
    .iconsearch {
      position: absolute;
      top: 50%;
      transform: translateY(-50%);
      right: 5%;
    }
  }
  .category_tab_content {
	height: calc(100vh - 36px);
	display: flex;
	flex-wrap: wrap;
	.cate_item {
		width: 33.33%;
		border: 5rpx solid #fff
		image {}
	}
  }
}
</style>
