<template>
  <view class="home_tab">
    <view class="home_tab_title">
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
      
      <view class="home_tab_content">
          <view v-if="current === 0">
              <home-recommend></home-recommend>
          </view>
          <view v-if="current === 1">
              <home-category></home-category>
          </view>
          <view v-if="current === 2">
              <home-new></home-new>
          </view>
          <view v-if="current === 3">
              <home-ablum></home-ablum>
          </view>
      </view>
  </view>
</template>
<script>
import homeAblum from './home-ablum';
import homeCategory from './home-category';
import homeNew from './home-new';
import homeRecommend from './home-recommend';
import { uniSegmentedControl } from '@dcloudio/uni-ui'
export default {
  components: {
    homeAblum,
    homeCategory,
    homeNew,
    homeRecommend,
    uniSegmentedControl
  },
  data() {
    return {
      items: [
        {title: "推荐"},
        {title: "分类"},
        {title: "最新"},
        {title: "专辑"},
      ],
      current: 0
    }
  },
  methods: {
    onClickItem(e) {
      if(this.current !== e.currentIndex) {
        this.current = e.currentIndex
      }
    }
  },
  onLoad() {
    // https://service.picasso.adesk.com/v3/homepage/vertical
    // 1. 原生的微信小程序api
    // wx.request({
    //   url: "https://service.picasso.adesk.com/v3/homepage/vertical",
    //   success(res) {
    //     console.log(1, res)
    //   }
    // })

    // 2. uni 
    // uni.request({
    //   url: "https://service.picasso.adesk.com/v3/homepage/vertical"
    // })
    // .then(res => {
    //   console.log(2, res)
    // })

    // 3.自己封装的请求
    this.request({
      url: "https://service.picasso.adesk.com/v3/homepage/vertical"
    })
    .then(res => {
      console.log(2, res)
    })
  }
}
</script>

<style lang="scss">
.home_tab {
  .home_tab_title {
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
  .home_tab_content {

  }
}
</style>