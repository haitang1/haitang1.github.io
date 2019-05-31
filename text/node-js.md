# node.js

# 简介  
Node.js是运行在服务端的JavaScript。Node.js是一个基于Chrome JavaScript运行时建立的一个平台。  

# 创建一个应用  
如果我们使用PHP来编写后端的代码时，需要Apache 或者 Nginx 的HTTP 服务器，并配上 mod_php5 模块和php-cgi。
从这个角度看，整个"接收 HTTP 请求并提供 Web 页面"的需求根本不需 要 PHP 来处理。  
不过对 Node.js 来说，概念完全不一样了。  
使用 Node.js 时，我们不仅仅 在实现一个应用，同时还实现了整个 HTTP 服务器。事实上，我们的 Web 应用以及对应的 Web 服务器基本上是一样的。  
Node.js 应用是由哪几部分组成的：  
1. 引入required模块：我们可以使用require指令来载入Node.js模块。
2. 创建服务器：服务器可以监听客户端的请求，类似于Apache、Nginx等HTTP服务器。
3. 接受请求与响应服务器：服务器很容易创建，客户端可以使用浏览器或中断发送HTTP请求，服务器接受请求后返回响应数据。  

## 步骤一、引入required模块  
使用require指令来载入http模块，并将实例化的HTTP赋值给变量http，实例如下：  
```javaScript
var http = require("http");
```

## 步骤二、创建服务器  
接下来我们使用*http.createServer()*方法创建服务器，并使用listen 方法绑定8888端口。函数通过request，response参数来接收和响应数据。  
在项目的根目录下创建一个*server.js*的文件，并写入以下代码：  
```javascript
var http = require('http');

http.createServer(function (request, response){
    //发送HTTP头部
    //HTTP状态值：200 ： ok
    //内容类型：text/plain
    response.writeHead(200, {'Conetnt-Type': 'text/plain'});

    //发送响应数据 "Hello World"
    respose.end('Hello World\n');
}).listen(8888);

//终端打印如下信息
consele.log('Server running at http://127.0.0.1:8888/');
```

以上代码我们完成了一个可以工作的HTTP服务器。  
使用node命令执行以上代码：  
```javascript
node server.js
Server running at http://127.0.0.1:8888/
```

打开浏览器访问http://127.0.0.1:8888/，会看到一个写着“Hello World”的网页。  
![nodejs-helloworld](/images/nodejs-helloworld.jpg)  

### 分析Node.js的HTTP服务器：  
* 第一行请求(require)Node.js自带的http模块，并且把它赋值给http变量。
* 接下来我们调用http模块提供的函数：createServer。这个函数会返回一个对象，这个对象有一个叫做listen的方法，这个方法有一个数值参数，指定这个HTTP服务器监听的端口号。  

# NPM的使用  
npm是随同NodeJs一起安装的包管理工具，能解决NodeJS代码部署上的很多问起，常见的使用场景有以下几种：  
* 允许用户从NPM服务器下载别人编写的第三方包到本地使用。
* 允许用户从NPM服务器下载并安装别人编写的命令行程序到本地使用。
* 允许用户将自己编写的包或命令行程序上传到NPM服务器供别人使用。  

安装的是旧版本的 npm，可以通过 npm 命令来升级，命令如下：  
```javascript
npm install npm -g
```
使用淘宝镜像：
```javascript
cnpm install npm -g
```

# 使用npm命令安装模块  
npm 安装 Node.js 模块语法格式如下：
```javascript
$ npm install <Module Name>
```

以下实例，我们使用 npm 命令安装常用的 Node.js web框架模块 express:
```javascript
$ npm install express
```

安装好之后，express 包就放在了工程目录下的 node_modules 目录中，因此在代码中只需要通过 require('express') 的方式就好，无需指定第三方包路径。
```javascript
var express = require('express');
```

# 全局安装与本地安装  
npm 的包安装分为本地安装（local）、全局安装（global）两种  
```javascript
npm install express          // 本地安装
npm install express -g   // 全局安装
```  

如果出现以下错误：
```javascript
npm err! Error: connect ECONNREFUSED 127.0.0.1:8087 
```

解决办法为：
```javascript
$ npm config set proxy null
```

## 本地安装  
1. 将安装包放在 ./node_modules 下（运行 npm 命令时所在的目录），如果没有 node_modules 目录，会在当前执行 npm 命令的目录下生成 node_modules 目录。
2. 可以通过 require() 来引入本地安装的包。

## 全局安装  
1. 将安装包放在 /usr/local 下或者你 node 的安装目录。
2. 可以直接在命令行里使用。  

## 查看安装信息  
你可以使用以下命令来查看所有全局安装的模块：
```
$ npm list -g
```

如果要查看某个模块的版本号，可以使用命令如下：  
```
$ npm list grunt
```

# 使用package.json  
packge.json 位于模块的目录下，用于定义包的属性。  

## Package.json 属性说明  
* name - 包名。
* version - 包的版本号。
* description - 包的描述。
* homepage - 包的官网 url 。
* author - 包的作者姓名。
* contributors - 包的其他贡献者姓名。
* dependencies - 依赖包列表。如果依赖包没有安装，npm 会自动将依赖包安装在 node_module 目录下。
* repository - 包代码存放的地方的类型，可以是 git 或 svn，git 可在 Github 上。
* main - main 字段指定了程序的主入口文件，require('moduleName') 就会加载这个文件。这个字段的默认值是模块根目录下面的 index.js。
* keywords - 关键字

# 卸载模块 
我们可以使用以下命令来卸载 Node.js 模块。
```
$ npm uninstall express
```

卸载后，你可以到 /node_modules/ 目录下查看包是否还存在，或者使用以下命令查看：
```
$ npm ls
```

# 更新模块 
我们可以使用以下命令更新模块：
```
$ npm update express
```

# 搜索模块 
使用以下来搜索模块：
```
$ npm search express
```

# 创建模块 


# NPM常用命令 
* NPM提供了很多命令，例如install和publish，使用npm help可查看所有命令。  
* 使用npm help <command>可查看某条命令的详细帮助，例如npm help install。
* 在package.json所在目录下使用npm install . -g可先在本地安装当前命令行程序，可用于发布前的本地测试。
* 使用npm update <package>可以把当前目录下node_modules子目录里边的对应模块更新至最新版本。
* 使用npm update <package> -g可以把全局安装的对应命令行程序更新至最新版。
* 使用npm cache clear可以清空NPM本地缓存，用于对付使用相同版本号发布新版本代码的人。
* 使用npm unpublish <package>@<version>可以撤销发布自己发布过的某个版本代码。  

# 使用淘宝NPM镜像  
可以使用淘宝定制的 cnpm (gzip 压缩支持) 命令行工具代替默认的 npm:
```
$ npm install -g cnpm --registry=https://registry.npm.taobao.org
```

这样就可以使用 cnpm 命令来安装模块了：  
```
$ cnpm install [name]
``` 

# Node.js Web 模块  

## 什么是Web服务器？  
Web服务器一般指网站服务器，是指驻留于因特网上某种类型计算机的程序，Web服务器的基本功能就是提供Web信息浏览服务。他只需支持HTTP协议、HTML文档格式及URL，与客户端的网络浏览器配合。  
大多数web服务器多支持服务端的脚本语言（PHP、Python、Ruby）等，并通过脚本语言从数据库获取数据，将结果返回给客户端浏览器。  

## Web 应用架构  
![web应用架构](/images/web_architecture.jpg)  

* Client - 客户端，一般指浏览器，浏览器可以通过 HTTP 协议向服务器请求数据。
* Server - 服务端，一般指 Web 服务器，可以接收客户端请求，并向客户端发送响应数据。
* Business - 业务层， 通过 Web 服务器处理应用程序，如与数据库交互，逻辑运算，调用外部程序等。
* Data - 数据层，一般由数据库组成。

