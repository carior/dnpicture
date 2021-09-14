# test

## Project setup
```
npm install
```

### Compiles and hot-reloads for development
```
npm run serve
```

### Compiles and minifies for production
```
npm run build
```

### Customize configuration
See [Configuration Reference](https://cli.vuejs.org/config/).

创建项目
vue create -p dcloudio/uni-present-vue my-project

vscode插件地址
https://www.bilibili.com/read/cv5560418

uniapp 文档地址
https://uniapp.dcloud.io/api/README

插件市场
https://ext.dcloud.net.cn/plugin?id=55

npm地址
https://www.npmjs.com/package/@dcloudio/uni-ui

接口地址
http://service.picasso.adesk.com/v3/homepage/vertical?limit=10&order=hot&skip=2

接口文档
https://www.showdoc.com.cn/414855720281749?page_id=3678621017219602

github参考项目
https://github.com/Cringe-z/dnpicture

小程序基础
b站上搜 “微信小程序 黑马程序员”

[简书笔记地址](https://www.jianshu.com/p/3dec2cc2e30b)

```js
<template>
  <view></view>
</template>

<script>
export default {
  
}
</script>

<style>

</style>
```
### 分段器 uniSegmentedControl
uni.setNavigationBarTitle 修改页面的标题
### 封装自己的异步请求
- 原生的请求不支持promise
- uni-api的请求不能够很方便的额添加请求中的效果
- uni-api的请求返回值是个数组 不方便

- 基于原生的promise来封装
- 挂载到Vue的原型上
- 通过this.request 的方式来使用

让图片自适应 `mode="widthFix"`
<!-- text 使用 可以换行-->
页面触底 上拉加载下一页事件 全屏滚动 `onReachBottom `
### 页面跳转
- navigator
```js
onLoad(options) {
	this.id = options.id
},
```
### moment.js的使用
### “热门”列表的基于scroll-view 的分页加载
- 使用 scroll-view 标签
- 绑定滚动条 触底事件 scrolltolower
- 实现分页逻辑
- 如果是scroll-view 而且还是flex布局 需要在标签上加上enable-flex
### 全屏滚动
页面触底 上拉加载下一页事件 全屏滚动 onReachBottom
### 专辑组件的实现
- 使用 setNavigationBarTitle 修改 页面标题
### 图片详情
- 封装 超链接组件
	- 缓存图片详情页面所需要滑动的 图片数 和 图片索引
	- 跳转到图片详情页面
- 发送请求数据
- 使用moment处理特殊时间格式
- 封装 手势滑动组件
- 下载图片
### 手势封装的思路
- 手指在容器上产生了移动
	- 手指按下屏幕事件 touchstart
	- 手指离开屏幕事件 touchend
	- 手指在屏幕上的坐标 event.changedTouches[0].client 和 clientY
- 手指在容器上滑动的时间不能太长
	- 记录按下屏幕的时间
	- 记录离开屏幕的时间	
- 根据坐标判断滑动的方向
	- 手指离开屏幕的是根据坐标判断滑动的方向
### 下载图片到本地
- downloadFile 下载远程文件到小程序的内存中
- saveImageToPhotosAlbum 将图片从内存中下载到本地
### 视频
- video标签上加 objectFit="fill" 撑满
- video标签的muted属性实现声音的开关
- button 的 open-type 设置为share 实现转发