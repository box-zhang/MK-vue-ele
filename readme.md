#技术栈

ele

vue-resource 和后端进行数据交互 ajax通信

vue-router 做前端路由，实现单页面应用

better-scroll 第三方js库

webpack 构建工具

es6 + eslint 	eslint  es6的代码风格检查工具



工程化、组件化、模块化

---



## 近几年前端开发趋势

1. 旧浏览器逐渐淘汰，移动端需求增加
2. 前端交互越来越多，功能越来越复杂
3. 架构从传统后台MVC向REST API+前端MVVM迁移

MVVM框架：View-ViewModel-Model

- 针对具有复杂交互逻辑的前端应用
- 提供基础的架构抽象
- 通过Ajax数据持久化，不需要刷新页面，只需要改动dom里需要改动的那部分数据和内容，特别是移动端场景，保证前端用户体验

<img src="../../../../Library/Application Support/typora-user-images/image-20200709145500997.png" alt="image-20200709145500997" style="zoom:25%;" />

Vue.js

2014年创建，vue定位起初并不是框架

什么是vue.js

- 它是一个轻量级的MVVM框架
- 是一个数据驱动+组件化的前端开发
- Github超过160k+的star数，并且社区完善 

对比Angular.js   React.js

- vue 更轻量，容易上手，学习曲线平稳
- vue吸取了两家之长，借鉴了angular的指令和react的组件化

###vue的核心思想

1. 数据驱动

   DOM是数据的一种自然映射，省去了手动操作dom获取数据的这个过程

   <img src="../../../../Library/Application Support/typora-user-images/image-20200709152417205.png" alt="image-20200709152417205" style="zoom:30%;" />

   

   > **数据响应原理**：我们有一份数据a.b，在vue对象实例化的过程中，会给a.b这份数据通过ES5的属性添加一个getter和setter，同时vuejs会对模版做编译，解析生成一个指令对象（黄色框里面的v-text指令），每个指令对象都会关联一个watcher，当我们对指令a.b做求值的时候，就会触发他的getter，这时候就会把依赖升级到这个watcher里面，当我们再次改变了a.b的值的时候，就会触发他的setter，会通知到对应关联的watcher ，这时，watcher就会再次对a.b求值，计算对比新旧值，当发现值改变了，watcher就会通知到指令，调用指令的update方法，由于指令是对DOM的封装，所以他就会调用原生DOM的方法，去更新视图。

   这样就完成了数据改变到数据更新的一个过程

   ![image-20200709152935021](../../../../Library/Application Support/typora-user-images/image-20200709152935021.png)

   

2. 组件化

   组件化的目的：扩展html元素，封装可用代码

   ![image-20200709154524843](../../../../Library/Application Support/typora-user-images/image-20200709154524843.png)

   组件设计原则：

   - 页面上每个独立的可视/可交互区域视为一个组件
   - 每个组件对应一个工程目录，组件所需要的各种资源在这个目录下就近维护
   - 页面不过是组件的容器，组件可以嵌套自由组合形成完整的页面



## vue-cli介绍

vue-cli是一个帮助我们写好vue.js基础代码的工具（到github去看他的readme），包括：

- 目录结构
- 本地调试
- 代码部署
- 热加载
- 单元测试

####安装

~~~shell
#查看是否安装node npm
#1.打开终端，执行以下命令安装Homebrew
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
brew install node   //sudo 使用admin权限
#安装成功后，查看node和npm版本
node -v
npm -v

# 安装vue-cli,若你一开始是安装脚手架是用  npm install -g @vue/cli  这个命令行，现在会报错，要更改成------》就是在命令行输入   npm install @vue/cli -g  就安装成功了，在这之前，先清掉缓存 npm cache clean --force
npm cache clean --force
npm install @vue/cli -g

#检查vue版本（大写V）
vue -V
vue list

## 创建项目&&选择配置创建项目的参数
vue init webpack sell

#清除当前目录下整个 node_modules
rm -rf node_modules
#清除目录下的package-lock.json，而不是package.json
rm package-lock.json
#强行清除npm缓存（非必须），使用命令
npm cache clear --force

rm package-lock.json && rm -rf node_modules && rm -rf ~/.node-gyp

#重新安装node_modules代码依赖，进入项目目录
npm install

##启动
npm run dev

~~~



## VUE介绍

### index.html	项目的入口文件

js和css会被动态插入到 index.html 这个页面

~~~html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width,initial-scale=1.0">
    <title>sell</title>
  </head>
  <body>
    <div id="app"></div>
    <!-- built files will be auto injected -->
  </body>
</html>

~~~

### main.js	项目的入口js

~~~javascript
// The Vue build version to load with the `import` command
// (runtime-only or standalone) has been set in webpack.base.conf with an alias.
import Vue from 'vue'
import App from './App'		//和import App from './App.vue'是一样的，但是为了简便通常不会带后缀名 
import router from './router'

Vue.config.productionTip = false	
//Vue.config.productionTip = false阻止显示生产模式（npm run build 打包之后给后端放在服务端上用的）的消息。如果没有这行代码，或者设置为true，控制台就会多出这么一段代码。
//You are running Vue in development mode.
//Make sure to turn on production mode when deploying for production.
//大概意思就是：你运行的Vu是开发模式。为生产部署时，请确保启用生产模式。

/* eslint-disable no-new */
new Vue({
  el: '#app',		//挂载点，挂载到id为app的元素上
  router,
  components: { App },	//注册了一个当前的app插件
  template: '<App/>'
})

~~~

### App.vue

~~~javascript
//每个组件分为三个部分:telement模版、script支持模型、style样式

<template>
  <div id="app">
    <img src="./assets/logo.png">
    <router-view/>
  </div>
</template>

<script>
export default {
  name: 'App'
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
~~~



### 

### 组件

要通过components做注册，然后才可以用

## webstorm配置

1. 修改javascript识别版本（原因：main.js中，ES6语法报错）

   preferences/Languages & Frameworks/javascript/下拉选择ECMAScript 6

2. 