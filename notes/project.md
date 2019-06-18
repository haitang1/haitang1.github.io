# 项目介绍

## 项目描述  
项目名称为东骏科技-Mes系统，此运营平台分为基础管理、物料管理、生产管理、系统报表三个大的模块，每个模块下还有不同的功能模块，在前端是以一个左侧栏展示出三个大的模块，大的模块中内嵌所包含的子模块。在内容展示区域设置了一个Tab标签页，每次点击菜单中的模块，都会在标签页中新增一个标签，能在同一个页面中切换之前操作过的模块。在本公司的框架中有一个系统功能模块，中间有一个菜单管理功能，菜单管理中能够添加各个级别的菜单。每个菜单添加一个唯一的URL，在编写各个功能的页面中通过关联URL，就能把编写的功能模块引用到页面中。  

使用的开发工具是Visual Studio，使用的框架是vue cli,使用elementUi组件库进行页面布局，使用了vue.js、typescript，在系统的各功能模块中有许多表格的增、删、改和表单的查询、校验。在框架中开发人员已经编写了一套基于elemenUi中部分组件的组件。能够在自己的框架中更方便的实现布局和功能。在一个增、删、改页面中使用elemenUI组件库中table组件，Form表单组件，自定义的Container容器组件。在功能的实现上使用了vue.js和typescript，使用axios与后台进行数据交互。  

为了更有条理的编写代码，框架中使用了webpack打包系统，分别编写不同功能的模块，通过引用实现不同的功能。  

##  webpack.build.conf.js  文件结构  
此文件为`webpack`的基础配置。  
```javascript
module.exports = {
entry: {
    app: '../src/modules/index.ts'  //定义项目的入口文件，可以定义多个入口
},
output: {
			publicPath: "../../modules/",  //
			path: join(__dirname, "../dist/modules"),//打包后的文件存放的地方
			filename:`App/index.js`, //打包后生成的文件名字
			chunkFilename:`App/[name].[contenthash].js`,//打包后输出文件的文件名
			libraryTarget: "umd",
			library:`webpack_modules_App`, //
			jsonpFunction:`webpackJsonp_App`
        },
        resolve: {                  //解析模块的路径
			modules:[join(__dirname,'../src/common/less'),join(__dirname,'../node_modules')], //指定搜索的目录
			extensions: ['.js', '.vue', '.json', '.ts'],  //自动解析文件的扩展名
			alias: {                //创建引用路径的别名
				'vue$': 'vue/dist/vue.esm.js',
				'@': resolve('src'),
				"tsservice":resolve("src/service"),
				
			}
        },
        module: {                   //定义处理不同类型模块的规则
			rules: [
				{
					test: /(\.ts)$/,        //匹配特定的条件，一般是一个正则表达式
					use: [{                 //指定代码通过定义的loader进行转换
						loader: "babel-loader",
						options: {
							presets: [
								"es2015", "stage-2"
							],
							"plugins": ["transform-es2015-modules-umd"]
						}
					},
					{
						loader: 'ts-loader',
						options: {
							appendTsSuffixTo: [/\.vue$/]
						}
					}],
					exclude: /node_modules/
				},
				{
					test: /\.(less|css)/,
					use: [
						{
								loader: "style-loader"
						}, {
								loader: "css-loader",
								options: {
										modules: false
								}
						}, {
								loader: "postcss-loader"
						}, {
								loader: "less-loader"
						}
					]
				},
				{
					test: /\.vue$/,
					loader: 'vue-loader'
				},{
					test: /\.(ttf|eot|svg)(\?v=[0-9]\.[0-9]\.[0-9])?$/,
					loader: "url-loader?limit=8192&name=assets/[name].[hash].[ext]"
				},{
					test: /\.(png|jpe?g|gif)$/i,
					loaders: [
					'url-loader?limit=8192&name=img/[name].[hash].[ext]',
					'image-webpack-loader?{progressive:true, optimizationLevel: 7, interlaced: false, pngquant:{quality: "65-90", speed: 4}}'
					]
				}
            ]
            plugins: [              //定义webpack插件插件
			new VueLoaderPlugin(),
			new AppendPlugin(moduleId,`__loadOver&&__loadOver(webpack_modules_${moduleId});`),
			new webpack.BannerPlugin('版权所有，翻版必究:'+new Date())
		]
		},
```  
> webpack 的基本配置文件中通过定义入口文件，输出打包文件，解析各个模块的路径，在项目文件中引用小的模块形成相互依赖的关系，在编写代码时结构更加的清晰。  

## webpack.dev.confi.js  

 webpack开发环境配置,构建开发本地服务器  

```javascript
module.exports = function () {
	return {
		devServer: {  //webpack-dev-server服务器配置
			clientLogLevel: 'error',  //console 控制台显示的消息，可能的值有 none, error, warning 或者 info
			disableHostCheck: true,
			hot: false,//是否开启热加载
			contentBase: [join(__dirname, '../static')],
			compress: false,  //是否开启压缩
			host: 'localhost', //主机名地址
			port: 9001, 
			inline:true,
			open: false,  //是否自动打开浏览器
			overlay: { warnings: false, errors: true }, //// warning 和 error 是否要显示
			publicPath: "/modules", //设置该目录下的包可以被访问
			proxy: {  //代理设置，用于前后端分离
				'/socket.io':{
					secure:true,
					changeOrigin:true, //告诉服务器从哪里提供内容,devServer.publicPath 将用于确定应该从哪里提供 bundle.
					ws: true,
					target:'https://iot.ktuntech.com'
				},
				[`/modules/sys/index.js`]:{  //请求到 /modules/sys/index.js 现在会被代理到请求 http://localhost:9001/modules/sys/index.js。
					secure:true, //运行在 HTTPS 上,接受无效证书
					changeOrigin:true,
					target:'http://localhost:'+devPort,
					pathRewrite: {[`/modules/${debugModule}/index.js`]: [`/modules/${debugModule}/${debugModule}.js`]}
				},  
				[`!/modules/${debugModule}/**/*`]:{
					secure:true,
					changeOrigin:true,
					target:/*'http://localhost:9000'*/'https://iot.ktuntech.com'
				}
			},//config.dev.proxyTable,
			quiet: true, // necessary for 89
			watchOptions: {  
				poll: false
			}
		},
		...D(debugModule,false),
		mode: "development",
		devtool: 'cheap-module-eval-source-map'
	}
}
```
> 