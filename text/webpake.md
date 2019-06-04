# webpake
`引用文献:`
* [webpack文档](https://www.webpackjs.com/concepts/)

# 概念  
本质上，`webpack`是一个现代javaScript应用程序的静态模块打包器(module bundler)。当webpack处理应用程序时，他会递归的构建一个依赖关系图(dependency graph)，其中包含应用程序需要的每个模块，然后将所有这些模块打包成一个或多个`bundler`。  

四个核心概念：  
* 入口(entry)
* 输出(output)
* loader
* 插件

## 入口[entry]   
入口起点(entry point)指示`webpack`应该使用哪个模块，来作为构建其内部依赖图的开始，进入入口起点后，`webpack`会找出有哪些模块和库是入口起点(直接或间接)依赖关系的。  

每个依赖项随即被处理，最后输出到称之为`bundles`的文件中，  

可以通过在`webpack`配置*entry*属性，来指定一个入口起点(或多个入口起点)。默认值为*./src*。  

**webpack.config.js**  
```javascript
module.exports = {
    entry: './path/to/my/entry/file.js'
};
```

## 出口[output]  
output属性告诉webpack在哪里输出他所创建的bundles，以及如何命名这些文件，默认值为`./dist`。基本上，整个应用程序结构，都会编译到你指定的输出路径的文件夹中。你可以通过在配置中指定一个**output**字段来配置这些处理过程：  

`webpack.config.js`  
```javascript  
const path = require('path');

module.exports = {
    entry:'./path/to/my/entry/file.js',
    output:{
        path: path.resolve(__dirname, 'dist'),
        filename: 'my-first-webpack.bundle.js',
    }
};
```

在上面的实例中，我们通过*output.filename*和*output.path*属性，来告诉*webpack bundle*的名称，以及我们想要bundle生成(emit)到哪里。path模块是一个Nodes.js 核心模块，用于操作文件路径。  

## loader  
loader让webpack能够去处理那些非JavaScript文件（webpack自生只理解JavaScript）。loader可以将所有类型的文件转换为webpack能够处理的有效模块，然后你就可以利用webpack的打包能力，对他进行处理。  

本质上，webpack loader将所有类型的文件，转换为应用程序的依赖图（和最终的bundle）可以直接引用的模块。  

*注意，loader 能够 import 导入任何类型的模块（例如 .css 文件），这是 webpack 特有的功能，其他打包程序或任务执行器的可能并不支持。我们认为这种语言扩展是有很必要的，因为这可以使开发人员创建出更准确的依赖关系图。*  

在更高的层面上，webpack的配置中loader有连个目标：  
1. **test** 属性，用于标识出应该被对应的loader进行转换的某个或某些文件。
2. **use** 属性，表示进行转换时，应该使用哪个loader。  

webpack.config.js  
```javascript
const path = require('path');

const confit = {
    output: {
        filename: 'my-first-webpack.bundle.js'
    },
    module:{
        rulse:[
            {test: /\.txt$/, use:'raw-loader'}
        ]
    }
};

module.exports = config;
```  
以上配置中，对一个单独的module对象定义了*rules*属性，里边包含了两个必须属性：**test**和**use**。这告诉webpack编译器（compiler）如下信息：  

> “嘿，webpack 编译器，当你碰到「在 require()/import 语句中被解析为 `'.txt'` 的路径」时，在你对它打包之前，先使用 raw-loader 转换一下。”  

*重要的是要记得，在webpack配置中定义loader时，要定义在**module.rules** 中，而不是**rules**。然而，在定义错误时webpack会给出严重的警告。*  

## 插件[plugins]  
loder 被用于转换某些类型的模块，而插件则可以用于执行范围更广的任务。插件的范围包括，从打包优化和压缩，一直到重新定义环境中的变量。差简接口功能极其强大，可以用来处理各式各样的任务。  

想要一个插件，只需要require()他，然后将它添加到plugins数组中。多数插件可以通过选项（option）自定义。你也可以在一个配置文件中因为不同目的而多次使用同一个插件，这时需要通过使用**new**操作符来创建他的一个实例。  

webpack.config.js  
```javascript
const HtmlWebpackPpugin = require('html-webpack-plugin'); //通过npm安装
const webpack = require('webpack');// 用于访问内置插件

const config = {
    modul:{
        rules:[
            {test: /\.txt$/, use: 'raw-loader' }
        ]
    },
    plugins:[
        new HtmlWebpackPlugin({template: './src/index.html'})
    ]
}
```  
webpack提供许多开箱可用的插件！查阅[插件列表](https://www.webpackjs.com/plugins)获取更多信息。  
## 模式  
通过选择**development**或**production**之中的一个，来设置mode参数，你可以启用相应模式下的webpack内置的优化  

```javascript
module.exports = {
    mode:'production'
}
``` 
# 模块  

在模块化编程中，开发者将程序分解成离散功能块（discrete chunks of functionality）,并称之为模块。  
每个模块具有比完整程序跟小的接触面，使得校验，测试，调试轻而易举。精心编写的模块提供了可靠的封装界限，使得应用程序中的每个模块都具有条理清楚的设计和明确的目的。  

Node.js从一开始就支持模块化编程。然而，在web，模块化的支持正缓慢到来。在web存在多种支持JavaScript模块的工具，这些工具各有优势和限制。webpack基于从这些系统后的的经验教训，并将模块的概念应用于项目中的任何文件。  

## 什么是webpack模块  

对比Node.js 模块，webpack模块能够以各种方式表达他们的依赖关系，  
* [ES2015](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/import) **import** 语句  
* [CommonJs](http://www.commonjs.org/specs/modules/1.0/) **require** 语句
* [AMD](https://github.com/amdjs/amdjs-api/blob/master/AMD.md) **define** 和 **require** 语句
* css/sass/less 文件中的 **@import** [语句](https://developer.mozilla.org/en-US/docs/Web/CSS/@import)。
* 样式(**url()**) 或 HTML 文件(<img src=...>)中的图片链接(img url)

> webpack 1 需要特定的 loader 来转换 ES 2015 import，然而通过 webpack 2 可以开箱即用。

## 支持的模块类型  
webpack 通过loader 可以支持各种语言和预处理器编写模块。 loader 描述了 webpack 如何处理非JavaScript(no-jacaScript)模块，并且在bundle中引入这些依赖。webpack社区已经为各种流行语言处理器构建了loader，包括：  
* [CoffeeScript](http://coffeescript.org/)
* [TypeScript](https://www.typescriptlang.org/)
* [ESNext (Babel)](https://babeljs.io/)
* [Sass](http://sass-lang.com/)
* [Less](http://lesscss.org/)
* [Stylus](http://stylus-lang.com/)

总的来说webpack提供了可定制的、强大和丰富的API，允许任何技术栈使用webpack，保持了在开发、测试和生产流程中无侵入性（non - opinionated）。  

# 模块解析[module resolution]  
resolver是一个库(library),用于帮助找到模块的绝对路径。一个模块可以作为另一个模块的依赖模块，然后被后者引用，如下：  
```javascript
import foo from 'path/to/module'
//或者
require('path/module')
```  

所依赖的模块可以是来自应用程序代码或第三方的库(library)。resolve帮助webpack找到bundle中需要引入的模块代码，这些代码在包含每个**require**/**import** 语句中。当打包模块时，**webpack** 使用 [enhanced-resolve](https://github.com/webpack/enhanced-resolve)来解析文件路径  

## webpack中的解析规则  

使用**enhanced-resolve** , webpack能够解析三种文件路径：  

### 绝对路径  
```javascript
import "/home/me/file";

import "C:\\Users\\me\\file";
```  
由于我们已经取的文件的绝对路径，因此不需要进一步做解释。  

### 相对路径  
```javascript
import "../src/file1";
import "./file2";
```  
在这种情况下，使用**import** 或 **require**的资源文件(resource file)所在的目录被认为是上下文目录(context directory)。在**import/require** 中给定相对路径，会添加此上下文路径(context path),以产生模块的绝对路径(absolute path)。  

### 模块路径  
```javascript
import "module";
import "module/lib/file";
```  
模块将在[resolve.modules](https://www.webpackjs.com/configuration/resolve/#resolve-modules) 中指定的所有目录内搜索。你可以替换初始模块路径，此替换路径通过使用[resolve.alias](https://www.webpackjs.com/configuration/resolve/#resolve-alias) 配置选项来创建一个别名。  

一旦根据上述规则解析路径后，解析器(resolver)将检查路径是否指向文件或目录。如果有路径指向同一文件：  

* 如果路径具有文件扩展名，则被直接将文件打包。  
* 否则，将使用（resolve.extensions）选项作为文件扩展名来解析，此选项告诉解析器在解析中能够接受那些扩展名(例如 .js , .jsx)。  

如果路径指向一个文件夹，则采取以下步骤找到具有正确扩展名的正确文件：
* 如果文件夹中包含package.json 文件，则按照顺序查找 resolve.mainFields 配置选项中指定的字段。并且 package.json 中的第一个这样的字段确定文件路径。 
* 如果 package.json 文件不存在或者 package.json 文件中的main字段没有返回一个邮箱路径，则按照顺序查找  resolve.mainFiles 配置选项中指定的文件名，看是否能在 import/require 目录下匹配到一个存在的文件名。
* 文件扩展名通过 resolve.mainFiles 选项采用类似的方法进行解析。  

### 解析Loader[Resolving Loaders]  
Loader 解析遵循与文件解析器指定的规则相同的规则。但是 [resolveLoader](https://www.webpackjs.com/configuration/resolve/#resolveloader) 配置选项可以用来为Loader提供独立的解析规则。  

### 缓存  
每个文件系统访问都被缓存，以便更快触发对同一文件的多行或串行请求。在观察模式下，只有修改过的问你哪会从缓存中摘出。如果关闭观察模式，在每次编译前清除缓存。  

# 入口起点[entry points]  
在webpack配置文件中有多种方式定义 **entry** 属性。  

## 单个入口（简写）语法  

用法：entry：string | Array<string>
webpack.config.js  
```javascript
const config = {
    entry: "./path/to/my/entry/file.js"
};

module.exports = config;
```  

entry 属性的单个入口语法，是下面的简写：  
```javascript
const config = {
    entry:{
        main: './path/to/my/entry/file.js'
    }
}
```  
> 当你向 *entry* 传入一个数组时会发生什么？向 *entry* 属性传入 [文件路径(file path)数组] 将创建"多个主入口(multi-main entry)"。在你想要多个依赖文件一起注入，并将它们的依赖导向(graph)到一个"chunk"时，传入数组的方式就会很有用。  

## 对象语法  
用法：entry: {[entryChunkName: string]: string|Array<string>}  

webpack.config.js  
```javascript
const config = {
    entry:{
        app: './src/app.js',
        vendors: './src/vendors.js'
    }
};
```  

对象语法会比较繁琐。然而，这是应用程序中定义入口的最可扩展的方式。  

> “可扩展的 webpack 配置”是指，可重用并且可以与其他配置组合使用。这是一种流行的技术，用于将关注点(concern)从环境(environment)、构建目标(build target)、运行时(runtime)中分离。然后使用专门的工具（如 [webpack-merge](https://github.com/survivejs/webpack-merge)）将它们合并。  

## 多页面应用程序  
webpack.config.js  
```javascript
const config = {
    entry:{
        pageOne: './src/pageOne/index.js',
    pageTwo: './src/pageTwo/index.js',
    pageThree: './src/pageThree/index.js'
    }
}
```