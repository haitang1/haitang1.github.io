# 六月

# 6/5  
## JavaScript 模块系统: CommonJs、AMD、ES2015比较 
`引用`
[JavaScript Module Systems Showdown: CommonJS vs AMD vs ES2015](https://auth0.com/blog/javascript-module-systems-showdown/)

随着JavaScript开发变得越来越普遍，命名空间(namespaces)和依赖关系性(depedencies)变得越来越难以处理。以模块系统(module systems)的形式开发了不同的解决方案处理该问题。  

## 为什么需要JavaScript模块  
通常单独开发不同的软件，直到先前存在的软件需要满足某些需求为止。在将其他软件引入到项目的那一刻，他与新代码之间产生了依赖关系(dependency)。由于这些软件需要协同工作，因此他们之间最重要的是不会产生冲突。如果没有某种封装(encapsulation)，两个模块之间相互冲突只是时间问题。这是`C`库中元素通常带有前缀的原因之一:  
 ```c++
 # ifndef MYLIB_INIT_H
 # define MYLIB_INIT_H

 enum mylib_init_code{
    mylib_init_code_success,
    mylib_init_code_error
 };

 enum mylib_init_code mylib_init(void);

 //(...)

 #endif
 ```  
 封装(encapsulation)对于防止冲突和简化开发至关重要。  

 说到依赖关系(dependence)，在传统的客户端JavaScript开发中，他们是隐含的。换句话说，开发人员的工作就是确保在执行任何代码块时满足依赖关系。开发人员开需要确保以正确的顺序满足依赖关系(某些库的需求)。  

以下实例是[Backbone.js](https://github.com/jashkenas/backbone/blob/master/examples/todos/index.html)实例的一部分。脚本按正确的顺序手动加载：  

```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="utf-8">
        <title>Backbone.js Todos</title>
        <link rel="stylesheet" href="todos.css"/>
    </head>

    <body>
        <script src="../../test/vendor/json2.js"></script>
        <script src="../../test/vendor/jquery.js"></script>
        <script src="../../test/vendor/underscore.js"></script>
        <script src="../../backbone.js"></script>
        <script src="../backbone.localStorage.js"></script>
        <script src="todos.js"></script>
    </body>

    <!-- (...) -->

</html>
```  

随着JavaScript开发变得越来越复杂，依赖关系管理变得很麻烦。重构(refactoring)也受到损害：应该在哪里建立新的依赖关系以维持负载链(maintain proper)的正确顺序？  

JavaScript模块系统尝试处理这些问题。他们的诞生是为了使用不断增长的JavaScript环境。看看带来了哪些解决方案。  

## Ad-HOC 解决方案：揭示了模块模式  

大多数模块系统都是最新的。在他们可用之前，特定的编程模式开始被越来越多的JavaScript代码使用：揭示模块模式。  

```javascript
var myRevealingModule = (function()
{
    var privateVar = "Ben Cherry",
        publicVar ="Hey there";

    function privateFunction(){
        console.log("Name:" + privateVar);
    }

    function publicSetName( strName ){
        privateVar = strName;
    }

    funciton publicGetName(){
        privateFunction();
    }

    // Reveal public pointers to
    // private functions and properties
    return {
        setName: publicSteName,
        greeting: publicVar,
        getName: publicGetName
    };
})();

myRevealingModule.setName( "Paul Kinlan" )
```
> 这个例子来自Addy Osmani的[JavaScript Design Patterns](https://addyosmani.com/resources/essentialjsdesignpatterns/book/)一书。

JavaScript范围(在ES2015中出现let前)在功能(function)级别工作。换句话说，在函数内声明的任何绑定都无法脱离其范围。正是由于这个原因，揭示模块(revealing module)依赖于函数来封装私有内容(与许多其他JavaScript模式一样)。  

在上面的实例中，公共符号在返回的字段中公开。所有包含它们的函数范围的其他其他声明都受保护。没有必要对包含私有范围的函数使用var和立即调用，命名函数也可用于模块。  

这种模式已经在JavaScript项目中使用了相当长的一段时间，并且很好的处理了封装问题。他对依赖性问题没有太大作用。类似的模块系统试图解决这个问题。另一个限制在于，包含其他模块不能再同一个源中完成(除非使用`eval`)。  

**优点**  
* 简单，可以在任何地方实现(没有哭，不需要语言支持)。
* 可以在单个文件中定义多个模块。

**缺点**  
* 无法以编程的方式导入模块(除非使用`eval`)。
* 需要手动处理依赖关系。
* 无法异步加载模块。
* 循环依赖可能很麻烦。
* 很难分析静态代码分析器。

## CommonJS  
commonJS 是一个旨在定义一系类规范的项目，以帮助开发服务器端JavaScript应用程序。CommonJS团队解决模块这个领域。Node.js 开发人员最初打算遵循CommonJS规范，但后来决定反对它。说到模块，Node.JS的实现很受它的影响：  
```javascript
// In circle.js
const PI = Math.PI;
export.area = (r) => PI * r * r;
exoprt.circumference = (r) => 2 * PI * r;

//In some file
const ciecle = require('./circle.js');
console.log(`The area of a circle of radius 4 is ${circle.area(4)}`)
``` 
> One evening at Joyent, when I mentioned being a bit frustrated some ludicrous request for a feature that I knew to be a terrible idea, he said to me, "Forget CommonJS. It's dead. We are server side JavaScript." [NPM创建者Isaac Z. Schlueter引用Node.js创建者Ryan Dahl](https://github.com/nodejs/node-v0.x-archive/issues/5132#issuecomment-15432598)

Node.js的模块系统以抽象库的形式存在，弥补了Node.JS模块和CommonJS之间的差距。  

在Node.js和CommonJS的模块中，基本上有两个鱼模块系统交互的元素：`require` 和 `exports`。require是一个函数，可用于将符号从另一个模块导入当前作用域。传递给`require`的参数是模块的*id*。在Node的实现中，它是`node_modules`目录中模块的名称（或者，如果它不在该目录中，则是它的路径）。`exports`是一个特殊的对象：放入它的任何东西都将作为公共元素导出。保留字段的名称。 Node和CommonJS之间的一个独特区别是以`module.exports`对象的形式出现。在Node中，`module.exports`是以真正特殊的对象导出，而`exports`只是一个默认绑定到`module.exports`的变量。另一方面，CommonJS没有`module.exports`对象。实际在Node中，无法通过`module.exports`导出完全构造的对象：  

```javascript
/// This won't work, replacing exports entirely breaks the binding to
// modules.exports.
exports =  (width) => {
  return {
    area: () => width * width
  };
}

// This works as expected.
module.exports = (width) => {
  return {
    area: () => width * width
  };
}
```

CommonJs模块的设计考虑了服务器开发。当然，API是同步的。换句话说，模块现在按照源文件中所需的顺序加载。  

**优点**  
* 简单：开发人员可以在不查看文档的情况下掌握概念。
* 集成了依赖管理：模块需要其他模块按照所需顺序加载。
* `require`可以在任何地方调用：模块可以通过编程的方式加载。
* 支持循环依赖。

**缺点**  
* 同步API使其不适合某些用途(客户端)。
* 每个模块一个文件。
* 浏览器需要加载程序库或转换。
* 模块没有构造函数(Node支持)。
* 很难分析静态代码分析器。

### 实现  
我们已经讨论过一种实现（部分形式）：Node.js.  

![NodeJS](../images/notes/Node.js_logo.svg)  

对于客户端目前有两种主流的选项:[webpack](https://webpack.js.org/)和[browserify](http://browserify.org/index.html)。Browserify明确开发用于解析类似Node的模块定义(许多Node包与它一起开箱即用！)并将代码以及来自这些模块的代码捆绑在一个包含所有依赖项的文件中。另一方面，webpack被开发用于在发布之前处理创建复杂的元转换途径。这包括将CommonJS模块捆绑在一起。  

### 异步模块定义（AMD）  

AMD诞生于一群对CommonJS采用的方向不满的开发人员。事实上，AMD在开发早期就与CommonJS分道扬镳。AMD和CommonJS之间的主要区别在于它支持异步模块加载。  

```javascript
//Calling define with a dependency array and a factory function
define(['dep1', 'dep2'], function (dep1, dep2) {

    //Define the module value by returning a value.
    return function () {};
});

// Or:
define(function (require) {
    var dep1 = require('dep1'),
        dep2 = require('dep2');

    return function () {};
});
```  

使用JavaScript的传统闭包语法可以实现异步加载：在请求的模块加载完成是调用函数。模块的定义和导入由相同的功能承载：当定义模块时，其依赖关系是明确的。因此，AMD加载器可以在运行时具有指定项目完整的依赖关系图。因此可以同时加载彼此彼此不依赖的库。这对于浏览器尤其重要，因为启动时间对于良好的用户体验至关重要。  

**优点**  
* 异步加载（更好的启动时间）。
* 支持循环依赖
* `require`和`exports`的兼容性。
* 完全整合了依赖管理。
* 如有必要，可以将模块拆分为多个文件。
* 支持构造函数。
* 插件支持（自定义加载步骤）。

**缺点** 
* 语法稍微复杂一些。
* 除非编译，否则需要加载程序库。
* 很难分析静态代码分析器。

### 实现  
目前最流行的AMD实现是[require.js](http://requirejs.org/)和[Dojo。](https://dojotoolkit.org/)  
![require.js](../images/notes/requirejs-logo.svg)  

使用require.js非常简单：在HTML文件中包含库，并使用data-main属性告诉require.js应首先加载哪个模块。 Dojo有[类似的设置](http://dojotoolkit.org/documentation/tutorials/1.10/hello_dojo/index.html)。  

## ES2015 Modules  
幸运的是，支持JavaScript标准化的ECMA团队决定解决模块问题。结果可以在最新版本的JavaScript标准中看到：ECMAScript 2015（以前称为ECMAScript 6）。结果在语法上令人愉悦并且兼容同步和异步操作模式。  

```javascript
//------ lib.js ------
export const sqrt = Math.sqrt;
export function square(x) {
    return x * x;
}
export function diag(x, y) {
    return sqrt(square(x) + square(y));
}

//------ main.js ------
import { square, diag } from 'lib';
console.log(square(11)); // 121
console.log(diag(4, 3)); // 5
```  
> Example taken from [Axel Rauschmayer blog](http://www.2ality.com/2014/09/es6-modules-final.html)  

`import`指令可用于将模块引入命名空间。与`require`和`define`相比，该指令不是动态的（即它不能在任何地方调用）。另一方面，`export`指令可用于显式地使元素公开。  

导入`import`和导出`export`指令的静态特性允许静态分析器在不运行代码的情况下构建完整的依赖树。 ES2015不支持动态加载模块，但规范草案确实：
```javascript
System.import('some_module')
      .then(some_module => {
          // Use some_module
      })
      .catch(error => {
          // ...
      });
```  

> 实际上，ES2015仅指定静态模块加载器的语法。实际上，在解析这些指令之后，ES2015实现不需要做任何事情。仍然需要System.js等模块加载器。可以使用浏览器模块加载规范草案。  

该解决方案通过集成在语言中，让运行时为模块选择最佳的加载策略。换句话说，当异步加载带来好处时，它可以由运行时使用。  

