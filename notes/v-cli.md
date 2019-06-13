# v-cli

## 使用vue-cli创建vue项目  
命令行输入
```
vue init webpack my-project
```
* Project name :项目名称 ，如果不需要更改直接回车就可以了。注意：这里不能使用大写。  
* Project description:项目描述，默认为A Vue.js project,直接回车，不用编写。
* Author：作者，如果你有配置git的作者，他会读取。
* Install vue-router? 是否安装vue的路由插件，Y代表安装，N无需安装，下面的命令也是一样的。
* Use ESLint to lint your code? 是否用ESLint来限制你的代码错误和风格。
* setup unit tests with Karma + Mocha? 是否需要安装单元测试工具Karma+Mocha。
* Setup e2e tests with Nightwatch?是否安装e2e来进行用户行为模拟测试。
* Should we run npm install for you after the project has been created? (recommended) npm 这一步会询问你使用npm安装还是yarn安装包依赖。  

## Vue-cli项目结果  
```
|-- build                            // 项目构建(webpack)相关代码
|   |-- build.js                     // 生产环境构建代码
|   |-- check-version.js             // 检查node、npm等版本
|   |-- utils.js                     // 构建工具相关
|   |-- vue-loader.conf.js           // webpack loader配置
|   |-- webpack.base.conf.js         // webpack基础配置
|   |-- webpack.dev.conf.js          // webpack开发环境配置,构建开发本地服务器
|   |-- webpack.prod.conf.js         // webpack生产环境配置
|-- config                           // 项目开发环境配置
|   |-- dev.env.js                   // 开发环境变量
|   |-- index.js                     // 项目一些配置变量
|   |-- prod.env.js                  // 生产环境变量
|-- src                              // 源码目录
|   |-- components                   // vue公共组件
|   |-- router                       // vue的路由管理
|   |-- App.vue                      // 页面入口文件
|   |-- main.js                      // 程序入口文件，加载各种公共组件
|-- static                           // 静态文件，比如一些图片，json数据等
|-- .babelrc                         // ES6语法编译配置
|-- .editorconfig                    // 定义代码格式
|-- .gitignore                       // git上传需要忽略的文件格式
|-- .postcsssrc                       // postcss配置文件
|-- README.md                        // 项目说明
|-- index.html                       // 入口页面
|-- package.json                     // 项目基本信息,包依赖信息等
--------------------- 

```  

## 引入elementUI  

```
npm i element-ui -S
```

## 安装less  
```
npm i --save-dev
```
## 创建一个登录页  

### 定义首页的路由  
通过`Vue-router`插件管理链接路径。文件在`src/router/index.js`路径。
```javascript
import Vue from 'vue'   //引入Vue
import Router from 'vue-router'  //引入vue-router
import Hello from '@/components/Hello'  //引入根目录下的Hello.vue组件
 
Vue.use(Router)  //Vue全局使用Router
 
export default new Router({
  routes: [              //配置路由，这里是个数组
    {                    //每一个链接都是一个对象
      path: '/',         //链接路径
      name: 'Hello',     //路由名称，
      component: Hello   //对应的组件模板
    }
  ]
})
```
### 引入element组件  

在`main.js`中写入以下内容：
```javascript
import Vue from 'vue'
import App from './App'
import router from './router'
import ElementUI from 'element-ui'  //引入element
import 'element-ui/lib/theme-chalk/index.css'  //引入element路径


Vue.use(ElementUI)  //全局使用element组件

/* eslint-disable no-new */
new Vue({
  el: '#app',
  router,
  components: { App },
  template: '<App/>',
  render: h => h(App)
})
```
### 增加一个登录后显示的页面  
* 在`src/components`目录下，新建Hi.vue文件。
* 在`router/index.js`引入Hi组件
```
import Hi from '@/components/Hi'
```
* 增加路由配置：在routes[]数组中
```
{
  path:'/hi',
  name:'Hi',
  component:Hi
}
```  

### 自定义添加公共组件  

把创建好的组件放到模板中  
```
import leftNav from '@/components/common/leftNav'
```
引入后在vue的构造器里添加components属性  
```
export default {
  name: 'app',
  components:{
    leftNav
  }
}
```
组件引入成功，可以在`<template>`区域使用`（<leftNav></leftNav>）`