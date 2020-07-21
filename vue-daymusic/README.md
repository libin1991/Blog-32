# vue-daymusic

> 音乐播放器

## Build Setup

``` bash
# install dependencies
npm install

# serve with hot reload at localhost:8080
npm run dev

# build for production with minification
npm run build

# build for production and view the bundle analyzer report
npm run build --report
```



## 项目环境搭建

**安装脚手架**

```
npm install --global vue-cli
```

**安装webpack** 

```
npm install webpack -g
```

**创建vue项目工程**

```
vue init webpack vue-daymusic
```

**有些配置文件的选项**

```
Vue build 选择runtime
ESLint -->> 选择Standard
```

**测试是否能支持运行**

```
cd vue-daymusic
npm run dev
```

![](C:\Users\DayDay\Desktop\Blog\images\vue-daymusic\环境搭建成功.PNG)

出现这个即表示运行成功



## 项目文件说明

![项目说明](C:\Users\DayDay\Desktop\Blog\images\vue-daymusic\项目说明.png)

对于其他的文件配置未修改，Vue-cli基本上以及配置完全，接下来我们主要将的修改内容就是src这个目录进行修改的👇 所以，数字标记的是显示修改的内容。



**api文件**

主要是一个后台数据请求，比如axios，jsonp跨域请求类似的

**assets**

通用的资源，比如字体文件，图片，通用的JS逻辑代码

**stylus**

通用的样式文件

**store**

状态库，存放vuex相关代码



**导入资源**

分别是这次项目的fonts和stylus文件中的样式，

地址在点这里，stylus点这里



**安装对应的loader**

比如stylus-loader

```
npm install stylus-loader stylus --save-dev
```

**.eslintrc.js文件**

在rules中增加两个规则

```markdown
rules: {
    // allow async-await
    'generator-star-spacing': 'off',
    // allow debugger during development
    'no-debugger': process.env.NODE_ENV === 'production' ? 'error' : 'off',
    
    'eol-last':0, //文件以单一的换行符结束 - 关闭
    'space-before-function-paren': 0 //函数定义时括号前面要有空格 - 关闭
  }
```

**webpack.base.conf.js配置别名**

```
resolve: {
    extensions: ['.js', '.vue', '.json'],
    alias: {
      '@': resolve('src'),
      'common': resolve('src/common'),
      'components': resolve('src/components'),
      'base': resolve('src/base'),
      'api': resolve('src/api')
    }
  },
```

有人会好奇这个resolve是什么函数，我们看看这个文件中是怎么写的吧

```
function resolve (dir) {
  return path.join(__dirname, '..', dir)
}
```

就是相对路径的写法，所以有时候，看见别人路径写法很奇怪，就是因为设置了别名

不写的话，默认情况找不到该路径下的资源，就去node_modules模块下找



**package.json增加内容**

**babel-runtime**

babel-runtime用以提供编译模块的工具函数，启用插件babel-plugin-transform-runtime后，Babel就会使用babel-runtime下的工具函数。

`npm install --save babel-runtime`

**fastclick** 解决移动端点击300ms问题

`npm install --save fastclick `

将他们作为生成依赖

```
"dependencies": {
	// 其他的省略
    "babel-runtime": "^6.0.0",
    "vue": "^2.3.3",
    "vue-router": "^2.5.3",
    "vuex": "^2.3.1",
    "fastclick": "^1.0.6",
  },
```



```
"devDependencies": {
   	// 其他的省略
    "babel-polyfill": "^6.2.0",
}
```

**babel-polyfill** 是解决使用es6中的API 比如 Promise

`npm install --save-dev babel-polyfill `作为开发依赖



**main.js导入上面的包**

```
import 'babel-polyfill'

import fastclick from 'fastclick'

fastclick.attach(document.body)
```



## 页面骨架的开发

**页面入口+header组件的编写**

```vue
<template>
  <div class="m-header">
    <div class="icon"></div>
    <h1 class="text">Chicken Music</h1>
    <router-link tag="div" class="mine" to="/user">
      <i class="icon-mine"></i>
    </router-link>
  </div>
</template>

<script type="text/ecmascript-6">
export default {

}
</script>

<style scoped lang="stylus" rel="stylesheet/stylus">
  @import "~common/stylus/variable"
  @import "~common/stylus/mixin"

  .m-header
    position: relative
    height: 44px
    text-align: center
    color: $color-theme
    font-size: 0
    .icon
      display: inline-block
      vertical-align: top
      margin-top: 6px
      width: 30px
      height: 32px
      margin-right: 9px
      bg-image('logo')
      background-size: 30px 32px
    .text
      display: inline-block
      vertical-align: top
      line-height: 44px
      font-size: $font-size-large
    .mine
      position: absolute
      top: 0
      right: 0
      .icon-mine
        display: block
        padding: 12px
        font-size: 20px
        color: $color-theme
</style>
```



路由的配置+tab顶导组件开发

```vue
<template>
  <div class="tab">
    <router-link tag="div" class="tab-item" to="/recommend">
      <span class="tab-link">推荐</span>
    </router-link>
    <router-link tag="div" class="tab-item" to="/singer">
      <span class="tab-link">歌手</span>
    </router-link>
    <router-link tag="div" class="tab-item" to="/rank">
      <span class="tab-link">排行
      </span>
    </router-link>
    <router-link tag="div" class="tab-item" to="/search">
      <span class="tab-link">搜索</span>
    </router-link>
  </div>
</template>

<script type="text/ecmascript-6">
export default {
  
}
</script>

<style scoped lang="stylus" rel="stylesheet/stylus">
  @import "~common/stylus/variable"
  .tab
    display: flex
    height: 44px
    line-height: 44px
    font-size: $font-size-medium
    .tab-item
      flex: 1
      text-align: center
      .tab-link
        padding-bottom: 5px
        color: $color-text-l
      &.router-link-active
        .tab-link
          color: $color-theme
          border-bottom: 2px solid $color-theme
</style>
```


