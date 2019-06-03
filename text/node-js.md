# node.js  
### 引用文献：  
* [菜鸟驿站](https://www.runoob.com/nodejs/nodejs-tutorial.html)
* [Node.js](http://nodejs.cn/api/)

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
# node.js模块系统  
为了让Node.js的文件可以相互调用，Node.js提供了一个简单的模块系统。  
模块是Node.js应用程序的基本组成部分，文件和模块是一一对应的。一个node.js文件就是一个模块，这个文件可能是JavaScript代码、JSON或者编译过的C/C++扩展。
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

# 使用Node创建Web服务器  
Node.js 提供了 http 模块，http 模块主要用于搭建 HTTP 服务端和客户端，使用 HTTP 服务器或客户端功能必须调用 http 模块，代码如下：  
```javascript
var http = require('http');
```

实例：  
```javascript
var http = require('http');
var fs = require('fs');
var url = require('url');
 
 
// 创建服务器
http.createServer( function (request, response) {  
   // 解析请求，包括文件名
   var pathname = url.parse(request.url).pathname;
   
   // 输出请求的文件名
   console.log("Request for " + pathname + " received.");
   
   // 从文件系统中读取请求的文件内容
   fs.readFile(pathname.substr(1), function (err, data) {
      if (err) {
         console.log(err);
         // HTTP 状态码: 404 : NOT FOUND
         // Content Type: text/html
         response.writeHead(404, {'Content-Type': 'text/html'});
      }else{             
         // HTTP 状态码: 200 : OK
         // Content Type: text/html
         response.writeHead(200, {'Content-Type': 'text/html'});    
         
         // 响应文件内容
         response.write(data.toString());        
      }
      //  发送响应数据
      response.end();
   });   
}).listen(8080);
 
// 控制台会输出以下信息
console.log('Server running at http://127.0.0.1:8080/');
```

index.html 文件  
```html
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<title></title>
</head>
<body>
    <h1>我的第一个标题</h1>
    <p>我的第一个段落。</p>
</body>
</html>
```

执行 server.js 文件：
```
$ node server.js
Server running at http://127.0.0.1:8080/
```

在浏览器中打开地址：http://127.0.0.1:8080/index.html，显示如下图所示:
![shuchu](/images/6E0D2A5C-0339-4D61-858D-A4EEB5763D98.jpg)

执行 server.js 的控制台输出信息如下：  
```
Server running at http://127.0.0.1:8080/
Request for /index.html received.     #  客户端请求信息
```

## 使用 Node 创建 Web 客户端  
Node 创建 Web 客户端需要引入 http 模块,创建 client.js 文件  
```javascript
var http = require('http');
 
// 用于请求的选项
var options = {
   host: 'localhost',
   port: '8080',
   path: '/index.html'  
};
 
// 处理响应的回调函数
var callback = function(response){
   // 不断更新数据
   var body = '';
   response.on('data', function(data) {
      body += data;
   });
   
   response.on('end', function() {
      // 数据接收完成
      console.log(body);
   });
}
// 向服务端发送请求
var req = http.request(options, callback);
req.end();
```

执行 client.js 文件，输出结果如下：
```
$ node  client.js 
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<title>菜鸟教程(runoob.com)</title>
</head>
<body>
    <h1>我的第一个标题</h1>
    <p>我的第一个段落。</p>c
</body>
</html>
```

执行 server.js 的控制台输出信息如下：  
```
Server running at http://127.0.0.1:8080/
Request for /index.html received.   # 客户端请求信息
```  
# Node.js 文件系统  
Node.js提供了一组类似UNIX(POSIX)标准的文件操作API。Node导入文件系统模块(fs)语法如下所示：  
```
var fs = require("fs)
```

# 异步同步  
Node.js文件系统(fs模块)模块中的方法均有异步和同步版本，例如读取文件类容的函数有异步的fs.readFile()合同部的fs.readFileSync()。  
异步的方法函数最后一个参数为回调函数，回调函数的第一个参数包含了错误信息(error)。  
比起同步，异步方法性能更高，速度更快，没有阻塞。  

## 实例  
创建input.text文件，类容如下：
```
网站地址：www.haitng.com
文件读取实例
```

创建file.js文件，代码如下：  
```javascript
var fs = require("fs");

//异步读取
fs.readFile('input.text',function(err,data){
   if(err){
      return console.error(err);
   }
   console.log("异步读取："+ data.toString());
});

//同步读取
var data = fs.readFileSync('input.txt');
console.log("同步读取："+ data.tostring());

console.log("程序执行完毕。");
```  
结果如下：  
```
$ node file.js
异步读取：网站地址：www.haitng.com
文件读取实例

同步读取：网站地址：www.haitng.com
文件读取实例

程序读取完毕
``` 

# 打开文件  

## 语法  
一下为在异步模式下打开文件的语法格式：
```javaScript
fs.open(path, flags[, mode], callback)
``` 

## 参数  
参数使用说明如下：  
* **path** - 文件的路径。
* **flags** - 文件打开的行为。
* **mode** - 设置文件模式(权限)，文件创建默认权限为0666(可读，可写)。
* **callback** - 回调函数，带有两个参数如：callback(err,fd)。 
  
  *flags*参数可以是以下值：  
  
| flag | 描述                                              |
| :--- | :------------------------------------------------ |
| r    | 已读取模式打开文件。如果文件不存在抛出异常。      |
| r+   | 以读写模式打开文件。如果文件不存在抛出异常。      |
| rs   | 以同步的方式读取文件。                            |
| rs+  | 以同步的方式读取和写入文件                        |
| w    | 以写入模式打开文件，如果文件不存在则创建。        |
| wx   | 类似'w',但是如果文件的路径存在，则文件写入失败。  |
| W+   | 以读写模式打开文件，如果文件不存在则建立          |
| wx+  | 类似'w+',但是如果文件的路径存在，则文件写入失败。 |
| a    | 以追加模式打开文件，如果文件不存在则创建          |
| ax   | 类似'a'，如果文件路径存在，则文件追加失败。       |
| a+   | 以读取追加模式打开文件，如果文件不存在则建立。    |
| ax+  | 类似'a+'，如果文件路径存在，则文件读取追加失败。  |
## 实例  
创建 file.js 文件，并打开 input.txt 文件进行读写  
```javaScript
var fs require("fs");

//异步打开文件
console.log("准备打开文件！");
fs.open('input.txt', 'r+', function(err, fd){
   if(err){
      return console.error(err);
   }
   console.log("文件打开成功！");
});
```
以上代码执行结果如下：  
```
$ node file.js 
准备打开文件！
文件打开成功！
```

# 获取文件信息  
## 语法  
通过异步模式获取文件信息的语法格式：  
```javaScript
fs.stat(path, callback)
```

## 参数  
参数使用说明如下：  
* **path** - 文件路径。 
* **callback** - 回调函数，带有两个参数：(err,stats),*stats*是fs.Stats对象。  

fs.stat(path)执行后，会将stats类的实例返回给其回调函数。可以通过stats类中的提供方法判断文件的相关属性。例如判断是否为文件：  
```javascript
var fs = require('fs');

fs.stat('/Users/liuht/code/itbilu/demo/fs.js', function (err,stats){
   console.log(stats.isFile()); //true
})
```

stats类中的方法有：  

| 方法                      | 描述                                                                         |
| :------------------------ | :--------------------------------------------------------------------------- |
| stats.isFile()            | 如果是文件返回true，否则返回false。                                          |
| stats.isDirectory()       | 如果是目录返回true，否则返回false。                                          |
| stats.isBlockDevice()     | 如果是块设备返回 true，否则返回 false。                                      |
| stats.isCharacterDevice() | 如果是字符设备返回 true，否则返回 false。                                    |
| stats.isSymbolicLink()    | 如果是软链接返回 true，否则返回 false。                                      |
| stats.isFIFO()            | 如果是FIFO，返回true，否则返回 false。FIFO是UNIX中的一种特殊类型的命令管道。 |
| stats.isSocket()          | 如果是 Socket 返回 true，否则返回 false。                                    |

## 实例  
创建 file.js 文件：
```javascript
var fs = require("fs");

console.log("准备打开文件！");
fs.stat('input.txt',function (err, stats){
   if (err){
      return console.error(err);
   }
   console.log(stats);
   console.log("读取文件信息成功！");

   //检测文件类型
   console.log("是否为文件(isFile)?" + stats.isFile());
   console.log("是否为目录(isDirectory)?" + stats.isDirectoy());
});
```

代码执行的结果：  
```java
$ node file.js 
准备打开文件！
{ dev: 16777220,
  mode: 33188,
  nlink: 1,
  uid: 501,
  gid: 20,
  rdev: 0,
  blksize: 4096,
  ino: 40333161,
  size: 61,
  blocks: 8,
  atime: Mon Sep 07 2015 17:43:55 GMT+0800 (CST),
  mtime: Mon Sep 07 2015 17:22:35 GMT+0800 (CST),
  ctime: Mon Sep 07 2015 17:22:35 GMT+0800 (CST) }
读取文件信息成功！
是否为文件(isFile) ? true
是否为目录(isDirectory) ? false
```

# 写入文件  

## 语法  
语法格式：  
```javascript
fs.writeFile(file,data[,options], callback)
```
writeFile直接打开文件默认是**w**模式，所以如果问价存在，该方法写入的内容会覆盖旧的文件内容。  

## 参数  

参数使用说明如下：  
* **file** - 文件或文件描述符。
* **data** - 要写如文件的数据，可以是String(字符串)或Buffer(缓冲)对象。
* **option** - 该参数是一个对象，包含{encoding,mode,flag}。默认编码为utf8,模式为0666 ， flag为'W'
* **callback** - 回调函数，回调函数只包括错误信息参数(err),写入失败时返回。

## 实例  
创建 file.js 文件  
``` javascript
var fs = require("fs");

console.log("准备写入文件"); 
fs.writeFile('input.text','我是通过fs.writeFile写入文件的内容', function(err){
   if (err){
      return console.log(err);
   }
   console.log("数据写入成功！");
   console.log("-----------");
   console.log("读取写入的数据");
   fs.readFile('input.txt',function(err,data){
      if(err){
         return console.error(err);
      }
      console.log("异步读取数据："+ data.toString());
   });
});
```

结果如下：  
```java
$ node file.js 
准备写入文件
数据写入成功！
--------我是分割线-------------
读取写入的数据！
异步读取文件数据: 我是通 过fs.writeFile 写入文件的内容
```

# 读取文件  

## 语法  
读取文件的语法格式：
```javascript
fs.read(fd, buffer, offset, length, position, callback)
```  
## 参数  
参数使用说明：  
* **fd** - 通过fs.open()方法返回的文件描述符。
* **buffer** - 数据写入的缓冲区。
* **offset** - 缓冲区写入的写入偏移量。
* **length** - 要从文件中读取的字节数。
* **position** - 文件读取的起始位置，如果position的值为null，则会从当前文件指针的位置读取。  
* **callback** - 回调函数，有三个参数err,bytesRead,buffer,err为错误信息，bytesRead表示读取的字节数，buffer为缓冲区对象。

## 实例  
input.txt 文件内容为：  
```
地址为：www.github.com
```  
file.js文件：  
```javascript
var fs = require("fs");
var buf = new Buffer.alloc(1024);

console.log("准备打开已存在的文件！");
fs.open('input.txt','r+',function(err,fd){
   if(err){
      return console.error(err);
   }
   console.log("文件打开成功！");
   console.log("准备读取文件：");
   fs.read(fd,buf,0,buf.length,0,function(err,bytes){
      if(err){
         console.log(err);
      }
      console.log(bytes + "字节被读取");

      //仅输出读取的字节
      if(byts > 0){
         console.log(buf.slice(0,bytes).toString())
      }
   })
})
```  
运行结果：  
```java
$ node file.js 
准备打开已存在的文件！
文件打开成功！
准备读取文件：
42  字节被读取
地址：www.github.com
```  

# 关闭文件  
## 语法  
关闭文件的语法格式：  
```javascript
fs.close(fd, callback)
```  
该方法使用了文件描述符来读取文件。  

## 参数  
参数的使用说明：  
* fd - 通过fs.open()方法返回的文件描述符。
* callback - 回调函数，没有参数。

## 实例  
input.txt文件：  
```
网站地址：www.github.com
```  
创建file.js文件，代码如下所示：  
```javascript
var fs = require('fs');
var buf = new Buffer.alloc(1024);

console.log("准备打开文件！");
fs.open('input.txt','r+',function(err,fd){
   if(err){
      return console.error(err);
   }
   console.log("文件打开成功！");
   console.log("准备读取文件！");
   fs.read(fd,buf,0,buf.length,0,function(err,bytes){
      if(err){
         console.log(err);
      }
      console.log(bytes + "字节被读取");

      //仅输出读取的字节  
      if(bytes > 0){
         console.log(buf.slice(0,bytes).toString());
      }

      //关闭文件
      fs.close(fd, function(err){
         if(err){
            console.log(err);
         }
         console.log("关闭文件成功");
      });
   });
});
```  

运行结果如下：  

```java
准备打开文件！
文件打开成功！
准备读取文件！
地址：www.github.com
文件关闭成功
```  
# 截取文件  
## 语法  
截取文件的语法格式：  
```javascript
fs.ftruncate(fd, len, callback)
```  

## 参数  
参数的使用说明：  
* fd - 通过 fs.open()方法返回的文件描述符。
* len - 文件内容截取的长度。
* callback - 回调函数，没有参数。

## 实例  
input.txt文件内容：  
```
site:www.github.com
```  

创建file.js文件，  
```javascript
var fs = require("fs")
var buf = new Buffer.alloc(1024);

console.log("准备打开文件！");
fs.open('input.txt','r+',function(err,fd){
   if(err){
      return console.error(err);
   }
   console.log("文件打开成功！");
   console.log("截取10字节内的文件内容，超出部分将被去除");

   //截取文件
   fs.ftruncate(fd,10,function(err){
      if(err){
         console.log(err);
      }
      console.log("文件截取成功。");
      console.log("读取相同的文件");
      fs.read(fd, buf,0,buf.length,0,function(err, bytes){
         if(err){
            console.log(err);
         }
         //仅输出读取的字节  
         if(bytes > 0){
            console.log(buf.slice(0,bytes).toString());
         }

         //关闭文件
         fs.close(fd, function(err){
            if(err){
               console.log(err);
            }
            console.log("文件关闭成功！");
         });
      });
   });
});
```  

以上代码执行结果如下：  

```java
$ node file.js 
准备打开文件！
文件打开成功！
截取10字节内的文件内容，超出部分将被去除。
文件截取成功。
读取相同的文件
site:www.g
文件关闭成功
```
#  删除文件  
## 语法  
删除文件的语法格式：  
```javascript
fs.unlink(path,callback)
```  
## 参数  
参数使用说明如下：  
* **path** - 文件路径
* **callback** - 回调函数，没有参数。  

# 实例  
input.txt文件内容为：  
```
site:www.github.com
```  
file.js文件：  
```javascript
var fs = require("fs");

console.log("准备删除文件！");
fs.unlink('input.txt',function(err){
   if(err){
      return console.error(err);
   }
   console.log("文件删除成功！");
})
```  
以上代码执行结果如下：  
```java
$ node file.js 
准备删除文件！
文件删除成功！
```  

# 体会  
