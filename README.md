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

### 封装自己的异步请求
- 原生的请求不支持promise
- uni-api的请求不能够很方便的额添加请求中的效果
- uni-api的请求返回值是个数组 不方便

- 基于原生的promise来封装
- 挂载到Vue的原型上
- 通过this.request 的方式来使用