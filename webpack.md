---
title: wepback学习笔记 
date: 2017-03-24 19:55:59
tags: webpack
---
### webpack学习笔记
1.css-loader是用来处理.css文件，style-loader是通过在页面中插入style标签处理样式
```
//js文件中加载css文件
require('style-loader!css-loader!./hello.css')
//在命令行中通过module-bind处理css文件的加载
webpack hello.js hello.bundle.js --module-bind 'css=style-loader!css-loader'
```
2.命令行中参数
--module-bind 打包文件需要用到的loader
--watch 监听文件的变化并重新打包
--progress 显示打包进度
--display-modules 打包的所有的模块
--display-reason 模块打包原因
### webpack config
*在项目中创建webpack.congfig.js文件，在命令行中运行webpack，会默认执行webpack.config.js中的配置，如果webpack的配置文件不是webpack.config.js,在命令行中则需要配置文件的名字
```
//命令行
webpack --config webpack.dev.config.js
//可以用npm来给wepback执行的时候添加参数，在package.json
{
	'scripts':{
		'webpack':'webpack --config webpack.dev.config.js --progress --display-module --display-reasons --color'
	}
}
```
### webpack配置参数entry
```javascript
//一个入口文件,entry的值为字符串
module.exports = {
	entry:'./src/script/main.js',
	output:{
    	path:'./dist/js',
	 	filename:'bundle.js'
	}
}
//多个入口文件,entry的值为数组，将main.js和a.js打包在一起，生成bundle.js
module.exports = {
	entry:['./src/script/main.js','./src/script/a.js'],
		  output:{
		  path:'./dist/js',
		  filename:'bundle.js'
		  }
}
//多页面文件,entry的值为对象,name为entry中的key,hash为此次打包的hash值，chunkhash为文件的md5值，可以认为是文件的版本号
module.exports = {
	entry:{
		"main":"./src/script/main.js",
		"a":"./src/script/a.js"
	},
	output:{
		path:'./dist/js',
		//filename:'[name]-[hash].js'
		filename:'[name]-[chunkhash].js'
	}
}
```
### 自动化打包生成页面中的html，动态打包的文件名是不确定的
利用webpack的html插件来解决
```
//安装html-webpack-plugin插件
npm install html-webpack-plugin --save-dev

//使用html-webpack-plugin插件,webpack.dev.config.js
var htmlWebpackPlugin = require('html-webpack-plugin')

module.exports = {
entry:{
main:'./src/script/main.js',
a:'./src/script/a.js'
	  },
output:{
path:'./dist/js',
filename:'[name]-[chunkhash].js'
	   },
plugins:[
new htmlWebpackPlugin()
]
}
//此时执行npm run webpack 会在dist/js目录里生成一个引用a.js main.js的index.html文件，这个index.html并没有与项目根目录下创建的index.html有关联

//关联根目录下的index.html
module.exports = {
entry:{
main:'./src/script/main.js',
a:'./src/script/a.js'
	  },
output:{
path:'./dist/js',
filename:'[name]-[chunkhash].js'
	   },
plugins:[
			new htmlWebpackPlugin({
			template:'index.html'
					})
]
}
// 打包后生成的index.html,a.js main.js都在dist/js/目录下，需要的目录结构应为
dist---index.html js/a.js js/main.js,此时需要修改output的path，filenam配置

var htmlWebpackPlugin = require('html-webpack-plugin')
module.exports = {
	entry:{
		'main':'./src/script/main.js',
		'a':'./src/script/a.js'
	},
	output:{
		path:'./dist',
		filename:'js/[name]-[chunkhash].js'
	},
	plugins:[
		new htmlWebpackPlugin({
			filename:'index-[hash].html',//指定文件的名字
			template:'index.html', //模板的名称
			inject:'head', //js插入的位置
			title:'webpack is good', //给模板index.html模板传递变量
			date:new Date() //给模板传递date参数
		})
	]
}
//index.html引用htmlWebpackPlugin里的title
<title><%= htmlWebpackPlugin.options.title %></title>
<body><%= htmlWebpackPlugin.options.date %></body>

//在html中遍历出htmlWebpackPlugins的能获取的参数

		<div><%= htmlWebpackPlugin.options.date %></div>
		<% for(var key in htmlWebpackPlugin) { %>
		<%= key %>
		<% } %>

		<% for (var key in htmlWebpackPlugin.files){ %>
		<%= key%>: <%= JSON.stringify(htmlWebpackPlugin.files[key]) %>
		<% } %>
		
		<% for (var key in htmlWebpackPlugin.options){ %>
		<%= key%>: <%= JSON.stringify(htmlWebpackPlugin.options[key]) %>
		<% } %>
		
//htmlWebpackPlugin的key
		files
		
		options
	//htmlWebpackPlugin.files的key/value	
		publicPath: ""
		
		chunks: {"main":{"size":26,"entry":"js/main-41d4982751088424227f.js","hash":"41d4982751088424227f","css":[]},"a":{"size":18,"entry":"js/a-4f55e4d7524c54cdeb7a.js","hash":"4f55e4d7524c54cdeb7a","css":[]}}
		
		js: ["js/main-41d4982751088424227f.js","js/a-4f55e4d7524c54cdeb7a.js"]
		
		css: []
		
		manifest: 
		
	//htmlWebpackPlugin.options的key/value	
		
		template: "/home/marong/webpack-demo/node_modules/html-webpack-plugin/lib/loader.js!/home/marong/webpack-demo/index.html"
		
		filename: "index.html"
		
		hash: false
		
		inject: "head"
		
		compile: true
		
		favicon: false
		
		minify: false
		
		cache: true
		
		showErrors: true
		
		chunks: "all"
		
		excludeChunks: []
		
		title: "webpack is good"
		
		xhtml: false
		
		date: "2017-03-25T12:12:45.828Z"
		
```


```

var htmlWebpackPlugin = require('html-webpack-plugin')
module.exports = {
	entry:{
		'main':'./src/script/main.js',
		'a':'./src/script/a.js'
	},
	output:{
		path:'./dist',
		filename:'js/[name]-[chunkhash].js',
		publicPath:'http://cdn.com/js' //上线打包地址占位,打包后生成的js文件地址是以此开头
	},
	plugins:[
		new htmlWebpackPlugin({
			//filename:'index-[hash].html',
			filename:'index.html',
			template:'index.html',
			inject:fasle, //关闭自动注入js文件,手动在index.html中插入js
			minify:{ //对html文件进行压缩
			removeComments:true,//删除html中注释
			collapseWhitespace:true //删除html中空格
			},
			title:'webpack is good',
			date:new Date()
		})
	]
}
//index.html
<head>
<script src='<%= htmlWebpackPlugin.chunks.main.entry'></script>
<script src='<%= htmlWebpackPlugin.chunks.a.entry'></script>
</head>
//配置publicPath,生成的html
<script src="http://cdn.com/js/main-19a6af676da9ed429fb5.js"></script>
<script src="http://cdn.com/js/a-9b44f8a7189f4606479f.js"></script>

```

### 处理多页面应用的情况

假设有a.html b.html c.html三个页面
```
//webpack.dev.config.js配置
var htmlWebpackPlugin = require('html-webpack-plugin')
module.exports = {
	entry:{
		'main':'./src/script/main.js',
		'a':'./src/script/a.js',
		'b':'./src/script/b.js',
		'c':'./src/script/c.js'
	},
	output:{
		path:'./dist',
		filename:'js/[name]-[chunkhash].js',
		publicPath:'http://cdn.com/'
	},
	plugins:[
		new htmlWebpackPlugin({
			//filename:'index-[hash].html',
			filename:'a.html',
			template:'index.html',
			inject:false, 
			title:'This a.html',
			date:new Date()
		}),
		new htmlWebpackPlugin({
			//filename:'index-[hash].html',
			filename:'b.html',
			template:'index.html',
			inject:false, 
			title:'This is b.html',
			date:new Date()
		}),

		new htmlWebpackPlugin({
			//filename:'index-[hash].html',
			filename:'c.html',
			template:'index.html',
			inject:false, 
			title:'This is c.html',
			date:new Date()
		})
	]
}

//index.html模板中js都固定引入chunks.a.entry,所以生成的三个页面都会有a.js
//解决方案，可以在webpack配置文件中传递变量给模板，模板来判断引入哪个js,
//这种方式比较麻烦.html-webpack-plugin中能设置chunks的传递值(默认传递所有的chunks),只传递对当前页面有用的chunks

<!DOCTYPE html>
<html>
	<head>
		<meta charset='utf-8'>
		<title><%= htmlWebpackPlugin.options.title %></title>
		<script src="<%= htmlWebpackPlugin.files.chunks.main.entry %>"></script>
		<script src="<%= htmlWebpackPlugin.files.chunks.a.entry %>"></script>
	</head>
	<body>
		<!-- 文章主体start -->
		<h1>webpack title</h1>
	</body>
</html>
```
### 多页面应用通过设置chunks参数，不同页面通过同一个模板加载相应的js文件
```
//webpack.dev.config.js
var htmlWebpackPlugin = require('html-webpack-plugin')
module.exports = {
	entry:{
		'main':'./src/script/main.js',
		'a':'./src/script/a.js',
		'b':'./src/script/b.js',
		'c':'./src/script/c.js'
	},
	output:{
		path:'./dist',
		filename:'js/[name]-[chunkhash].js',
		publicPath:'http://cdn.com/'
	},
	plugins:[
		new htmlWebpackPlugin({
			//filename:'index-[hash].html',
			filename:'a.html',
			template:'index.html',
			inject:true,//打开自动注入js
			title:'This a.html',
			chunks:['main','a'] //对a.html有用的chunks
			//excludeChunks:['b','c']//如果页面中需要加载大多数chunks,可以采用排除的方式,排除当前页面不需要的chunks
		}),
		new htmlWebpackPlugin({
			//filename:'index-[hash].html',
			filename:'b.html',
			template:'index.html',
			inject:true, 
			title:'This is b.html',
			chunks:['b']
		}),
		new htmlWebpackPlugin({
			//filename:'index-[hash].html',
			filename:'c.html',
			template:'index.html',
			inject:true, 
			title:'This is c.html',
			chunks:['c']
		})
	]
}
//index.html
<!DOCTYPE html>
<html>
	<head>
		<meta charset='utf-8'>
		<title><%= htmlWebpackPlugin.options.title %></title>
	</head>
	<body>
		<!-- 文章主体start -->
		<h1>webpack title</h1>
	</body>
</html>
```
### 如果追求页面的性能，减少http的请求数，想在页面中以inline的形式插入js
html-webpack-plugin的作者给出了解决方法，在https://github.com/jantimon/html-webpack-plugin/blob/master/examples/inline/template.jade
中，利用webpack暴露出的变量compilation.assets在页面中插入chunks中的entry,由于webpack配置了publicPath，chunk的ernty都带着publicPath,
compilation.assert中存储的是不带publicPath的entry
```
<!DOCTYPE html>
<html>
	<head>
		<meta charset='utf-8'>
		<title><%= htmlWebpackPlugin.options.title %></title>
		<script>
<%= compilation.assets[htmlWebpackPlugin.files.main.entry.substr(htmlWebpackPlugin.files.publicPath.length)].source() %>
		</script>
	</head>
	<body>
		<!-- 文章主体start -->
		<h1>webpack title</h1>
	</body>
</html>
```
### loader
处理资源文件，返回新的资源。
loader的处理方式有三种，require加载文件相应文件的时，在webpack配置文件里，在命令行里。
```
//require的时候
require('style-loader!css-loader!./src/style/common.css')
//webpack的配置文件
{
module:{
loaders:[
		{
		test:/\.jade$/,loader:"jade"
		},
		{
		test:/\.css$/,loader:'style!css'
		},
		{
		test:/\.css$/, loaders:["style","css"]
		}
]
	   }
}
//命令行列
webpack --module-bind jade --module-bind 'css=style!css'
```
### webpack babel
处理es6语法，es6每年都会修订，es2015/es2016/es2017,latest插件包含所有特性(es2015/es2016/es2017)
通过preset设置babel-loader转换特性,插件需要通过npm来安装
给babel指定参数有四种方式，一种是在babel-loader后直接跟参数，一种是在webpack配置文件中通过query设置,
还可以通过创建.babelrc配置文件,在package.json中配置babel

设置babel-loader， exclude,include时候，需要绝对路径，并且同时设置，不然会报错
couldn't find preset latest
```
//安装babel
npm install --save-dev babel-loader bable-core
//安装latest插件
inpm install --save-dev  bable-preset-latest
//babel-loader直接跟参数
//配置文件中设置参数

var htmlWebpackPlugin = require('html-webpack-plugin')
var path  = require('path');
module.exports = {
	entry:'./src/app.js',
	output:{
		path:'./dist',
		filename:'js/[name].bundle.js'
	},
	module:{
		loaders:[
			{
				test:/\.js$/,
				loader:'babel',
				exclude:path.resolve(__dirname, 'node_modules'),//排除babel-loader编译文件夹
				include:path.resolve(__dirname, 'src'),//babel-loader很耗时，指定编译的文件夹
				query:{
					presets:["latest"]
				}
			}
		]
	},
	plugins:[
		new htmlWebpackPlugin({
			filename:'index.html',
			template:'index.html',
			inject:'body'
		})
	]
}

//.babelrc配置文件
{
"presets":["latest"]
}
//package.json配置babel

  "babel":{
	  "presets":["latest"]
  },
```


### webpack处理css
* postcss-loader css后处理loader，postcss常用插件autoprefixer
*style-loader css-loader  post-css
*如果在css文件中@import 'common.css', webpack会用style-loader,css-loader处理，但不会用postcss-loader来处理，
并把通过import引入的css新建一个style标签来插入到页面中
需要css-loader配置参数,importLoaders=num,设置在css-loader之后，指定几个数量的loader来处理import的css
```
npm install --save-dev style-loader css-loader postcss-loader autoprefixer
```
```
//webpack.dev.config.js

var htmlWebpackPlugin = require('html-webpack-plugin')
var path  = require('path');
module.exports = {
	entry:'./src/app.js',
	output:{
		path:'./dist',
		filename:'js/[name].bundle.js'
	},
	module:{
		loaders:[
			{
				test:/\.js$/,
				loader:'babel',
				exclude:path.resolve(__dirname, 'node_modules'),
				include:path.resolve(__dirname, 'src'),
				query:{
					presets:["latest"]
				}
			},
			{
				test:/\.css$/,
				loader:'style-loader!css-loader?importLoaders=1!postcss-loader'
			}
		]
	},
	//postcss:function(){//postcss是数组，填写postcss-loader需要的插件，必填也可直接写成一个数组的格式
		//return [
			//require('autoprefixer')({
				//broswers:['last 5 versions']
			//})
		//]
	//},
	postcss:[
		require('autoprefixer')({
			broswers:['last 5 versions']
		})
	],
	plugins:[
		new htmlWebpackPlugin({
			filename:'index.html',
			template:'index.html',
			inject:'body'
		})
	]
}

```
### webpack 处理less sass文件
如果是less文件中import less文件，能正常编译，不需要给css-loader配置importLoaders参数
```
npm install less less-loader sass-loader --save-dev
```
```
//webpack 配置
	module:{
		loaders:[
			{
				test:/\.js$/,
				loader:'babel',
				exclude:path.resolve(__dirname, 'node_modules'),
				include:path.resolve(__dirname, 'src'),
				query:{
					presets:["latest"]
				}
			},
			{
				test:/\.css$/,
				loader:'style-loader!css-loader?importLoaders=1!postcss-loader'
			},
			{
				test:/\.less/,
				//loader:'style-loader!css-loader!postcss-loader!less-loader'
				loader:'style!css!postcss!less'//可以省略-loader
			}
		]
	},
```
### webpack 处理项目中的模板文件
*有多种模板文件loader, html-loader, ejs-loader
```
//html-loader把模板处理为字符串
npm install --save-dev html-loader ejs-loader
```
```
//html

<!DOCTYPE html>
<html>
	<head>
		<meta charset='utf-8'>
		<title><%= htmlWebpackPlugin.options.title %></title>
	</head>
	<body>
		<!-- 文章主体start -->
		<h1>webpack title</h1>
		<div id="app"></div>
	</body>
</html>
//lader.html

<div class="layer">
	<div>This is a layer</div>
</div>
//layer.tpl

<div class="layer">
	<div><%= name %></div>
	<% for (var i = 0; i < arr.length;i++) {%>
<%= arr[i] %>
	<% } %>
</div>
//layer.js

//import tpl from './layer.html';
import tpl from './layer.tpl';
import './layer.less';
function layer(){
	return {
		name:'layer',
		tpl:tpl
	}

}
export default layer;
//app.js

import './css/common.css';
import Layer from './components/layer/layer.js';
const App = function(){
	var layer = new Layer();
	var app = document.getElementById('app');
	//html string
	app.innerHTML = layer.tpl;
	//ejs function 
	app.innerHTML = layer.tpl({
		name:'ejs',
		arr:['tpl','html', 'loader']
	})

}
new App();
//wepback配置文件

var htmlWebpackPlugin = require('html-webpack-plugin')
var path  = require('path');
module.exports = {
	entry:'./src/app.js',
	output:{
		path:'./dist',
		filename:'js/[name].bundle.js'
	},
	module:{
		loaders:[
			{
				test:/\.js$/,
				loader:'babel',
				exclude:path.resolve(__dirname, 'node_modules'),
				include:path.resolve(__dirname, 'src'),
				query:{
					presets:["latest"]
				}
			},
			{
				test:/\.css$/,
				loader:'style-loader!css-loader?importLoaders=1!postcss-loader'
			},
			{
				test:/\.less$/,
				//loader:'style-loader!css-loader!postcss-loader!less-loader'
				loader:'style!css!postcss!less'
			},
			{
				test:/\.tpl/,
				loader:'ejs-loader'
			},
			{
				test:/\.html$/,
				loader:'html-loader'
			}
		]
	},
	//postcss:function(){
		//return [
			//require('autoprefixer')({
				//broswers:['last 5 versions']
			//})
		//]
	//},
	postcss:[
		require('autoprefixer')({
			broswers:['last 5 versions']
		})
	],
	plugins:[
		new htmlWebpackPlugin({
			filename:'index.html',
			template:'index.html',
			inject:'body'
		})
	]
}
```


### webpack 处理图片及其他文件
file-loader 处理图片
url-loader 当图片大小小于limit的时候，转为base64,大于limit的时候，交由file-loader处理
image-webpack-loader 压缩图片大小，与file-loader,url-loader配合使用
```
npm install --save-dev file-loader url-loader image-webpack-loader

			{
				test:/\.(jpg|png|svg|gif)/i,
				//loader:'file-loader',
				//query:{
				//	name:'assets/[name]-[hash:5].[ext]'
				//}
				//loader:'url-loader',
				//query:{
				//	limit:100000,
				//	name:'assets/[name]-[hash:5].[ext]'
				//}
				loaders:[
					'url-loader?limit=100000&name=assets/[name]-[hash-5].[ext]',
					'image-webpack'
				]
			}

```
### webpack-dev-server
* webpack-dev-server是一个小型Node.js Express服务器，它使用webpack-dev-middleware中间件来为通过wepback打包生成的资源文件提供web服务。它还有一个通过Socket.IO连接着webpack-dev-server服务器的小型运行时程序。
* publicPath
* 启动webpack-dev-server后，目标文件中看不到编译后的文件，实时编译后的文件都保存到了内存中。

### postcss-sprites 自动拼合图片
能自动拼合图片，能生成单个或多个sprites图片，并能自动输出图片的大小
```
npm install --save-dev postcss-sprites
```
配置如下
```
var postcss = require('postcss')
var postcssSprites = require('postcss-sprites')
var postcssAssets = require('postcss-assets')
var postcssNext = require('postcss-cssnext')
var updateRule = require('postcss-sprites/lib/core').updateRule;

        new webpack.LoaderOptionsPlugin({
            options: {
                postcss:function(){
                    return [
                        postcssAssets({
                            loadPaths: ['./src/images/'],
                            relative: true
                        }),
                        postcssNext({//代替autoprefixer
                            browsers: ["> 1%", "last 2 version", "Android >= 4.0"]
                        }),
                        postcssSprites({
                        //需要处理样式表的文件夹
                            stylesheetPath: './src/css',
                            //生成sprites图片的文件夹
                            spritePath: './src/images/outputsprite',
                            outputDimensions: true,
                            skipPrefix: true,
                            //过滤需要合并的图片
                            filterBy: function(image) {
		                            if (!/sprite/.test(image.url)) {
			                              return Promise.reject();
		                            }

		                            return Promise.resolve();
	                          },
                              //可以根据匹配规则，合并某组文件为一个sprites
                            groupBy: function(image) {
                            //图片文件名中有shapes合并为一个sprite.shape.png
		                            // if (image.url.indexOf('shapes') === -1) {
			                          //     return Promise.reject(new Error('Not a shape image.'));
		                            // }

		                            // return Promise.resolve('shapes');
                                    //根据文件夹拼合一组图片，如果有/images/sprites/index   /images/sprites/login 这两个文件夹，会生成三张图片，sprites.index.png,sprites.login.png, sprites.other.png 三张图片

                                let groups = /\/images\/sprites\/(.*?)\/.*/gi.exec(image.url);
                                let groupName = groups ? groups[1] : 'other';
                                image.retina = true;
                                image.ratio = 1;
                                if (groupName) {
                                    let ratio = /@(\d+)x$/gi.exec(groupName);
                                    if (ratio) {
                                        ratio = ratio[1];
                                        while (ratio > 10) {
                                            ratio = ratio / 10;
                                        }
                                        image.ratio = ratio;
                                    }
                                }
                                return Promise.resolve(groupName);
	                          },
	                          hooks: {
                              //onUpdateRule控制输入到css的内容，下面的代码回在生成的css中带有图片的宽高
		                            onUpdateRule: function(rule, token, image) {
			                              // Use built-in logic for background-image & background-position
			                              updateRule(rule, token, image);

			                              ['width', 'height'].forEach(function(prop) {
				                                var value = image.coords[prop];
				                                if (image.retina) {
					                                  value /= image.ratio;
				                                }
				                                rule.insertAfter(rule.last, postcss.decl({
					                                  prop: prop,
					                                  value: value + 'px'
				                                }));
			                              });
		                            }
	                          }
                        })
                    ]
                },

```

### extract-text-webpack-plugin
把样式单独打包成一个样式文件,并插入到页面中原来注入style标签的位置,对css、less、sass文件配置
```

var ExtractTextPlugin = require('extract-text-webpack-plugin')
var ExtractTextPlugin = require('extract-text-webpack-plugin')

var DeployFtpWebpackPlugin = require('./deploy.plugin');
// process.traceDeprecation = true;
module.exports = {
    entry:'./src/main.js',
    output:{
        path: path.resolve(__dirname, 'dist'),
        filename: 'bundle-[hash].js'
    },
    module: {
        rules: [
            {
                test:/\.js$/,
                loader:'babel-loader',
                exclude: /node_modules/
            },
            {
                test:/\.css$/,
                use: ExtractTextPlugin.extract({
                    fallback: 'style-loader',
                    use:'css-loader?importLoaders=1!postcss-loader'
                })
                // loader: 'style-loader!css-loader?importLoaders=1!postcss-loader'
            },
            {
                test:/\.scss/,
                use: ExtractTextPlugin.extract({
                    fallback: 'style-loader',
                    use: 'css-loader!postcss-loader!sass-loader'
                })
                // loader:'style-loader!css-loader!postcss-loader!sass-loader'
            },
            {
                test:/\.(jpg|png|svg|gif)$/,
                loader: 'url-loader',
                query: {
                    limit:10,
                    name: 'assets/[name]-[hash:5].[ext]'
                }
            }
        ]
    },
    plugins: [
        new ExtractTextPlugin("style.css"),
        ...
```

### 第三方库不打包处理,webpack.IgnorePlugin
```
var ignoreFiles = new Webpack,IgnorePlugin(/\/jquery.js$/);
module.exports = {
plugins: [ignoreFiles]
}
```


### 提取多入口文件中公共的代码，打包成一个独立的文件webpack.optimize.CommonsChunkPlugin
1. 可以提取多入口文件中公共的代码
```
    entry:{
        main:'./src/main.js',
        main2:'./src/main2.js',
        vender:Object.keys(packageJson.dependencies)
    },
    ...
        new webpack.optimize.CommonsChunkPlugin({
            name:'common',
            filename:'common.js',
            chunks: ['main', 'main2']
        }),
        //最后会生成三个文件common.js main.js main2.js vender.js

```

#### webpack.optimize.OccurenceOrderPlugin
* 给组件分配ID，通过这个插件webpack可以分析考虑使用最多的模块，并未他们分配最小的ID
在webpack2中改为webpack.optimize.OcurrenceOrderPlugin,在webpack2中默认开启,不用在配置文件中写 

plugins: [
    // webpack 1
-   new webpack.optimize.OccurenceOrderPlugin()
    // webpack 2
-   new webpack.optimize.OccurrenceOrderPlugin()
  ]

### wepback.optimize.UglifyJsPlugin压缩JS代码
```bash
npm install uglifyjs-webpack-plugin --save-dev
```
```javascript

```

### 遇到的问题
1. 配置loader时不能省略-loader
1. postcss配置报错，在webpack2中不能包含自定义配置项来配置loader，postcss用LoaderOptionsPlugin来给loader配置
报错信息
```bash
Invalid configuration object. Webpack has been initialised using a configuration object that does not match the API schema.
 - configuration has an unknown property 'postcss'. These properties are valid:
   object { amd?, bail?, cache?, context?, dependencies?, devServer?, devtool?, entry, externals?, loader?, module?, name?, node?, output?, performance?, plugins?, profile?, recordsInputPath?, recordsOutputPath?, recordsPath?, resolve?, resolveLoader?, stats?, target?, watch?, watchOptions? }
   For typos: please correct them.
   For loader options: webpack 2 no longer allows custom properties in configuration.
     Loaders should be updated to allow passing options via loader options in module.rules.
     Until loaders are updated one can use the LoaderOptionsPlugin to pass these options to the loader:
     plugins: [
       new webpack.LoaderOptionsPlugin({
         // test: /\.xxx$/, // may apply this only for some modules
         options: {
           postcss: ...
         }
       })
     ]

```
webpack2中需替换为
```javascript
        new webpack.LoaderOptionsPlugin({
            options: {
                postcss:function(){
                    return [
                        require('autoprefixer')({
                            broswers:['last 5 version']
                        }),
                        require('postcss-assets')({
                            loadPaths: ['./src/images/'],
                            relative: true
                        }),
                        require("postcss-cssnext")({
                            browsers: ["> 1%", "last 2 version", "Android >= 4.0"]
                        }),
                        require('postcss-sprites')({
                            stylesheetPath: './src/css',
                            spritePath: './src/images/sprite.png',
                            outputDimensions: true,
                            skipPrefix: true,
                            filterBy: function(img) {
                                return /\/sp\-/.test(img.url);
                            },
                            groupBy: function(img) {
                                var match = img.url.match(/\/(sp\-[^\/]+)\//);
                                return match ? match[1] : null;
                            }
                        })
                    ]
                },
            }
        })
```
