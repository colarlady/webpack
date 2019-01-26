# 为什么要有构建？

- 开发分工的变化

  - 前端负责的工作越来越多，文件可能越来越多
  - 以前前端开发和后台开发
    - 前端：View + View Logic
    - 后端:  Router Render DataLogic  DataBase
  - 现在前端开发和后台开发
    - 前端：View+View Logic+Router+Render+Data-Logic
    - 后端：DataLogic  +API+DataBase

- 框架的变化

  - JS库-----MVC ---MV*
  - JS库：提供方法解决浏览器之间兼容问题
  - MVC:backbone.js+underscore.js+jquery  已经有模块化和分层的思想
  - MVVM：react+Angular+Vue
  - 模块化开发需要构建

- 语言的变化

  - html1->html5
  - css1-css3-less/sass/stylus
  - vbscript---js--ts

- 环境的变化

  - 浏览器---node--移动端

- 社区的变化

  - github+npm

- 工具的变化

  - grunt gulp fis webpack rollup.js

    开发复杂化，框架去中心化（一个包解决一个问题）

# 为什么选择Webpack？

- vue-cli react-restart  webpack-cli
- 代码分割
- 天生模块化等等

# 模块化开发

## js模块化

- webpack支持AMD/CommonJS/ES Module

- 命名空间

  - 库名.类别名.方法名

  - ```javasc
    var NameSpace = {}  //命名空间
    //避免变量重复定义覆盖问题
    NameSpace.type = NameSpace.type||{}
    NameSpace.type.method = function(){
    }
    ```

  - 弊端1：需要记住很长一个路径似的变量名

  - 弊端2：需要团队提前协商好命名空间的问题

- commonJS规范（服务器端）

  - 一个文件是一个模块

  - 通过module.exports来暴露模块接口

  - 通过require来引入模块

  - 模块之间是同步执行的

    - 本地加载（不想前端是通过http请求去加载的）
    - 加载的时候就执行
    - 模块只第一次加载的时候执行，以后再加载返回第一次执行的结果

  - [详细介绍](http://wiki.commonjs.org/wiki/Modules/1.1.1)

  - ```javascript
    //引入模块
    var eventEmitter = require('event').EventEmitter
    var res = require('./response')
    //导出模块
    exports = module.exports = createApplication
    ```

  - ​

- AMD规范（前端）

  - Ascync Module Definition 异步模块定义

  - 使用define来定义模块

  - 使用require来加载模块

  - RequireJs是AMD规范推行的一个产物

  - [详细介绍](https://github.com/amdjs/amdjs-api/blob/master/AMD.md)

  - 依赖前置，提前执行

    - ```javascript
      define(
          //模块名
          "alpha",
          //依赖项
          ["require","exports","beta"],
          //模块输出
          function(require,exports,beta){
              exports.verb = function(){
                  return beta.verb();
                  
                  //require模块没有使用但是也会提前执行
              }
          }
      );
      ```

- CMD规范(前端)

  - Common Module Definition

  - 一个文件是一个模块

  - 使用define来定义一个模块

  - 使用require来加载一个模块

  - SeaJs是CMD规范的一个产物

  - 特点：尽可能懒加载

    - 就是模块会加载但是会在用到该模块的时候再去执行

    - ```javascript
      define(function(){
          //通过require来引入依赖
          var $ = require('jquery')
          
          //通过exports向外暴露一些接口
          exports.doSomethiing = 
          //通过 Module.exports提供整个接口
          module.exports = 
      });
      ```

  - [详细介绍](https://github.com/cmdjs/specification/blob/master/draft/module.md)

- UMD规范(前端)

  - Universal Module Definition  通用模块解决方案

  - 三个步骤

    - 是否支持AMD

    - 是否支持CommonJs

    - 如果都没有则将其定义为全局变量

    - ```javascript
      (function(root,factory){
          if(typeof define ==='function'& define.amd){
             		define([],factory);
             }else if(typeof exports === 'object'){
              	module.exports = factory();
          }else{
             root.returnExports = factory(); 
          }
      }(this,function(){
          return {};
      }))
      ```

- ES6 Module规范

  - ES6 Module规范和前面的都会有一些规范有一些冲突

  - 一个文件一个Module

  - import引入模块

  - export导出模块

  - ```javascript
    //引入default
    import theDefault,{named1,named2} from 'src/mylib'
    import theDefault  from 'src/mylib'
    import {named1,named2} from 'src/mylib'

    //引入模块可以给模块起别名
    import {named1 as myNamed1,named2} from 'src/mylib'

    //引入所有模块
    import * from 'src/mylib'

    //只是加载模块，不引入任何其暴露的接口
    import 'src/mylib'
    ```



    //导出
    
    ```

## css模块化

- CSS设计模式

  - OOCSS

    - Object Oriented CSS 面向对象的CSS
    - 设计和结构分离
    - 内容和容器分离

  - SMACSS

    - 啥的
    - Base:基本规则，网站的主要元素长样子
    - LayOut:布局规则
    - Module:模块内部的规则
    - State:元素的状态
    - Theme:主题

  - Atomic CSS

    - 原则CSS

    - 每个CSS都代表一个独特含义可以进行组合

      - ```javascript
        ml-100{
            margin-left:100px;
        }
        h-50{
            height:50px;
        }

        ```

      - ``

  - MCSS

    - 多层级的CSS
    - fundation
    - base 
    - project
    - cosmetic

  - AMCSS

    - 针对属性进行定义类名
    - `<div am-large="large" am-disabled></div>`

  - BEM

    - Block Element Modifier

    - Blcok

      - header,container,menu,checkbox,input

    - Element

      - menu item,list item,checkbox caption,head title

    - Modifier

      - disabled,highlighted,checked,fixed,sizebig,color yellow

    - 举个栗子

      - ```html
        <button class="button button-state-success">
            Success Button
        </button>
        .button{
        	display:inline-block;
        	border-radius:3px;
        	padding:7px 12px;
        	border:1px solid #D5D5D5;
        	background-image:linear-gradient(#EEE,#DDD);
        	font:700 13px/18px arial;
        }
        .button-state-success{
        	color:#fff;
        	...
        }
        ```

# 环境准备

## 命令行工具安装

- windows下是git bash  cmd
- mac:自带Terminal,也可以安装其他比如iTerm2,Zsh 

## 安装Node

## 安装webpack

- mac上安装会有权限问题，可以上官网查看如何解决

# Webpack学习

## webpack 概述

- loaders
- 打包
- 代码分割，按需加载

## webpack 版本更迭

- webpackV1
  - 编译，打包
  - HMR（hot module reload）模块热更新
  - 代码分割
  - 文件等资源处理（loader和plugin）
- WebpackV2
  - TreeShaking
    - 引入但是未使用的包会在打包的时候自动删除使得打包完的项目更小
  - ES Module
    - 支持ES module语法
  - 支持动态import
  - 新的官方文档
- WepackV3
  - Scope Hoisting(作用域提升）
    - 不是提升打包的性能而是打包以后代码运行的性能提升
    - webpackV2是将每一个模块单独包裹在一个闭包中，闭包越多性能越低
    - webpackV3是将所有模块的作用域提升到一个单一的闭包中
  - magic Comments(配合动态import使用)
    - 动态import是懒加载的方式加载模块
    - 这样会使得动态加载的模块打包之后的名称无法确定
    - magic Comments是可以指定动态打包后文件的名称

## wepack版本迁移

- V1-V2迁移
  - [参考文档]()
- V2-V3迁移
  - npm update
- V3-V4迁移
  - [参考文档]()

## webpack 核心概念

## webpack使用

wepack四个核心要素：

- Entry(打包入口)

  - 代码入口

  - 单个或者多个入口

    - 多页面应用程序(不同页面是一个entry)
    - 单页面应用程序（业务代码--entry  框架代码--entry）

  - 打包入口

    - 从哪个文件开始找依赖

  - 栗子（三种方式）

    - ```javascript
      module.exports = {
          //单个入口
          entry:'index.js'
      }
      ```

    - ```javascript
      module.exports = {
          //多个入口
          entry:['index.js','vendor.js']
      }
      ```

    - ```javascript
      module.exports = {
          entry:{
              //推荐这种对象的方式
              //1.每一个entry有一个对应的key
              //2.增加entry,增加key---value即可
              //3.
              index:'index.js',
              vendor：'vendor.js'
          }
      }
      ```

- Output（输出）

  - 打包生成的文件（bundle）

  - 输出到一个或者多个文件（entry有多个）

  - 多个entry和output的对应有对应的规则

  - 栗子

    - ```javascript
      module.exports = {
          entry:'index.js',
          output:'index.min.js'
      }
      ```

    - ```javascript
      module.exports ={
          entry:{
              index:'index.js',
              vendor:'vendor.js'
          },
          output:{
              filename:[name].min.[hash:5].js
          }
      }
      ```

      ​

- Loaders(处理js之外的其他文件)

  - 处理文件转化为module
    - 举个栗子CSS的loader
    - 使用css的loader将css文件转化为一个模块，可以在js文件中引入
  - 常用的loader
    - 编译相关loader
      - babel-loader ES6有关
      - ts-loader typescript有关
    - 样式相关loader
      - style-loader
      - css-loader
      - less-loader
      - sass-loader
      - stylus-loader
      - postcss-loader
    - 文件相关
      - file-loader
      - url-loader

- Plugins(依靠插件实现代码分割等等其他功能)

  - 作用
    - 参与打包整个过程
    - 打包优化与压缩
    - 配置编译时的变量
    - 非常灵活
  - 常用的plugins
    - 优化相关的
      - CommonsChunkPlugin
      - UglifyjsWebpackPlugin
    - 功能相关的
      - ExtractTextWebpack
      - HtmlWebpackPlugin
      - HotModuleReplacementPlugin
      - CopyWebpackPlugin

- 名词 

  - Chunk就是代码块
  - Bundle 打包后的文件
  - Module 模块

### Webpack命令

- webpack  -h
  - 查看webpack相关的命令以及配置
- webpack -v
  - 查看webpack的版本
- 打包
  - webpack <entry> [<entry>]  <output>
- webpack-cli
  - 先欠着

### Webpack配置

- webpack
  - webpack 
    - 直接去找项目下的
  - webpack --config  webpack.config.dev.js
    - 指定配置文件

### 第三方脚手架

- vue-cli
- angular-cli
- React-starter

### webpack打包JS

- 见代码3-2

### webpack配合Babel编译ES6

- 见代码3-3

- babel

  - babel-loader是一个编译性质的loader,需要词法分析语法转换

    - 安装

      - ```shell
        npm install babel-loader@8.0.0-beta.0 @babel/core --save--dev
        npm install babel-loader babel-core --save--dev
        ```

    - 配置

      - ```javascript
            module:{
                rules:[
                    {
                        test:'/\.js$/', //需要使用以下loader处理的文件
                        use:'babel-loader',
                        exclude:'/node_modules/'
                    }
                ]
            }
        ```

- babel-presets

  - babel-loader进行词法分析的时候需要一个参照的规范，babel-presets就是这个规范

  - 规范

    - es2015
    - es2016
    - es2017
    - env(包含15-17)
    - babel-preset-react
    - babel-preset-stage 0-3  

  - 安装

    - ```shell
      # 安装的是最新的babel
      	npm install @babel/preset-env --save--dev
      # 安装的不是最新的babel
      	npm install babel-preset-env --save--dev
      ```

  - 配置

    - ```javascript
          module:{
              rules:[
                  {
                      test:'/\.js$/', //需要使用以下loader处理的文件
                      use:{
                          loader:'babel-loader',
                          options:{
                              presets:['@babel/preset-env']
                          }
                      },
                      exclude:'/node_modules/'
                  }
              ]
          }
      ```

  - 相关plugins

    - 以上的配置只能是对语法转换，但是对于es6新增的函数是无法支持的

      - 比如 Array.from
      - Array.prototype.includes
      - Set
      - Map
      - Generator

    - 需要借助plugin

      - 选择依据

        - 如果是开发一个应用并且需要使用到ES的新方法使用Babel Polyfill
        - 如果是开发一个框架让别人使用，需要用到ES的新方法使用Babel Runtime Transform

      - Babel Polyfill(垫片)各个浏览器实现效果不一样我们需要平掉这个区别

        - 全局垫片
        - 只要引入，整个浏览器都会自动平掉，ES6那些新的方法都会被支持
        - 为开发一个应用而准备
        - 安装
          - `npm install babel-polyfill --save`
        - 引用
          - `import 'babel-polyfill' `

      - Babel Runtime Transform

        - 局部垫片

        - 为开发框架准备

        - 不会污染全局（只是在框架内支持）

        - 安装

          - 如果安装的是`babel-loader babel-core`
            - `npm install babel-runtime --save`
            - `npm install babel-plugin-transform-runtime --save--dev`
          - 如果安装的是`babel-loader@8.0.0-beta.0 @babel/core`
            - `npm install @babel/runtime --save`
            - `npm install @babel/plugin-transform-runtime --save--dev `

        - 使用

          - 新增一个.babelrc文件

          - 配置如下

            - ```json
              #.babelrc文件的本质是一个json文件，需要严格遵照json的语法格式
              {
                  "presets":[
                      ["@babel/preset-env",{
                      "targets":{
                          "browsers":["> 1%"]
                      }
                  }]
                  ],
                  "plugins":["@babel/transform-runtime"]
              }
              ```

  - 参数

    - targets

      - targets.bowsers  目标浏览器

        * ``last 2 versions`
        * `>1%`

      - 配置

        - ```javascript
                 rules:[
                     {
                         test:'/\.js$/', //需要使用以下loader处理的文件
                         use:{
                             loader:'babel-loader',
                             options:{
                                 presets:[
                                     ['@babel/preset-env',{
                                         targets:{
                                             browsers:['> 1%','last 2 versions']
                                         }
                                     }]
                                 ]
                             }
                         },
                         exclude:'/node_modules/'
                     }
                 ]
             }
             ```
          ```

          ```

### webpack打包公共代码

- 为什么要提取公共代码

  - 其他模块依赖公共模块
  - 减少代码冗余，每个页面都有公共代码
  - 提高用户的加载速度，公共模块已下载直接从缓存中获取即可
    - （A,c)和(B, c)这样c需要下载两次
    -   (A)-->(C)  B需要C直接从缓存中获取无需再下载

- 如何实现

  - 借助插件

    - CommonsChunkPlugin(这个是webpack自带的，我们需要将webpack下载到本地项目，全局那个是提供命令的)

  - 配置

    - ```shell
      {
          plugins:[
              new webpack.optimize.CommonsChunkPlugin(option)
          ]
      }
      ```

    - options

      - options.name或者options.names(数组)
        - 设置chunk的名称，一般是已存在的chunk的name，就会把公共模块代码合并到这个chunk上，否则的话会新建一个为该name的chunk，将公共代码合并到这个chunk上
        - 如果是names的话，就是新建多个对应名称的chunks实例
      - options.filename
        - 公共代码打包后的文件名
      - options.minChunks
        - 数字:代码出现几次会被提取出来当做公共代码
        - 函数：根据指定的函数自定义提取规则来提取公共代码
        - Inifinity：无穷大，不会提取公共代码
      - options.chunks
        - 指定从哪几个entry中提取公共代码,是一个数组
      - options.children
      - options.deepChildren
        - 指定是否从entry的子模块中查找公共依赖
      - options.async
        - 创建一个异步公共代码块

- 使用场景

  - 栗子：
    - PageA     引用subPageA  subPageB
    - PageB    引用subPageB
    - subPageA  引用moduleA
    - subPageB  引用moduleA
  - 页面应用+第三方依赖+webpack生成代码（webpack内置函数）

### webpack 实现代码分割

使用代码分割和懒加载实现性能优化，性能优化是在最短的时间加载最少的代码让用户尽可能早的看到页面

实现代码分割和懒加载并不是通过webpack的配置来实现，而是通过改变代码的编写来实现的

- 懒加载的实现

  两种方式：

  - webpack内置的方法

    - require.ensure()动态加载模块
      - 注意点：对原生promise有依赖，所以需要使用babel-polyfill
      - 参数一:dependencies
        - 加载依赖，但是不会执行依赖
        - 需要在callback中require一下ensure的代码才会执行
      - 参数二：callback
      - 参数三：errorcallback(可省略)
      - 参数四: chunkName
    - require.includes()
      - 只接收一个参数就是依赖模块
      - 只加载不执行
      - 单entry中需要提取业务公共代码块

  - es2015 loader 规范

    - 动态import

      - import()

      - import('模块名')返回值是一个promise

      - import('模块名').then() 执行回调函数（处理）

      - 配合magic comments来指定chunk名称,加载方式

        - 栗子：

          - ```javascript
            import(
            	/* webpackChunkName:async-chunk-name*/
            	/* webpackMode:lazy*/
            	.....
            )
            ```


            //region 动态import方式加载
            if(page==='subPageA'){
                import(/*webpackChunkName:subPageA*/'./subPageA').then(function(){
                    var _ = require('./subPageA')
                    console.log('subPageA');
                });
            }else if(page==='subPageB'){
                import(/*webpackChunkName:subPageB*/'./subPageB').then(function(){
                    var _ = require('./subPageB')
                    console.log('subPageB');
                });
            }
            ```

- 代码分割

  - 分离业务代码和第三方依赖

    - 栗子：

      - ```javasc
        import './subPageA.js'
        import './subPageB.js'

        // import * as _ from 'loadsh'

        //懒加载
        require.ensure(['loadsh'],function(){
            var _ = require('loadsh')
            _.join(['1','2'],'3')
        },'vendor');

        export default 'pageA'
        ```

      - 最终打包结果

        - pageA.bundle.js
        - vendor.chunk.js

  - 分离业务代码和业务公共代码 和第三方依赖

    - 栗子

      - ```javascript
        require.include('./moduleA')
        var page = 'subPageA'

        if(page==='subPageA'){
            require.ensure(['./subPageA'],function(){
                var _ = require('./subPageA')
            },'subPageA');
        }else if(page==='subPageB'){
            require.ensure(['./subPageB'],function(){
                var _ = require('./subPageB')
            },'subPageB');
        }

        // import * as _ from 'loadsh'

        //懒加载
        require.ensure(['loadsh'],function(){
            var _ = require('loadsh')
            _.join(['1','2'],'3')
        },'vendor');

        export default 'pageA'
        ```

  - 分类首次加载和访问后加载的代码

### webpack 处理css

- 如何处理CSS文件

  - 引入`style-loader`,`css-loader`

  - css-loader是处理如何让js可以import一个css

  - style-loader是处理页面如何引用这些样式，是生成一个link标签，将css代码放在其中，引入到页面上还是将样式放到style标签中引入到页面上

  - 安装

    - ```shell
      cnpm install style-loader --save
      cnpm install css-loader --save
      ```

  - 配置

    - ```javascript
       module: {
              rules: [
                {
                  test: /\.css$/,
                  use: [ 'style-loader', 'css-loader' ]
                }
              ]
          }
       ```
      ```

      ```

- style-loader

  - 配置参数

    - insertAt 插入位置

    - insertInto 插入到某一个dom

    - singleton  是否只使用一个style标签

    - transform 样式转换

    - 配置栗子：

      - ```javascript
         {
              loader:'style-loader',
              options:{
                  insertInto:'#app',
                  singleton:true,
                  transform:'./css.transform.js'
              }
          }
         ```
        ```

        ```

    - 解释transform

      - transform指向的是一个路径，这个路径文件暴露一个函数

      - 这个函数不是在webpack打包的时候执行，而是在style-loader将样式引入到页面上的时候执行，他执行的环境是浏览器，它是可以拿到浏览器相关的参数，根据这些参数对CSS进行一些操作

        - 栗子:

          - ```javascript
            module.exports = function(css){
                console.log(css);
                if(window.innerWidth>768){
                    return css.replace('red','green')
                }else {
                    return css.replace('red','yellow')
                }
            }
            ```

- css-loader

  - 配置参数

    - alias 配置解析的别名 （基本不用）
    - importLoader
      - 作用取决于css-loader后面是否还有其他的loader
    - minimize (是否压缩文件)
      - 注意最新版本的已经取消这个属性了
    - modules(是否启用css-modules)
    -  localIdentName:'[path][name]__[local]--[hash:base64:5]'  设置类名编码方式

  - 栗子

    - ```javascript
       options:{
               modules: true,
               localIdentName:'[path][name]__[local]--[hash:base64:5]'
        }
       ```
      ```

      ```

- 如何使用CSS module

  - CSS-modules语法

    - ：local  本地样式

    - ：global 全局样式

    - compose  继承一段样式

    - compose  ... from path  引入一个样式

    - 栗子：

      - ```javascript
        .box {
            width: 200px;
            height: 200px;
            border:2px;
            background: #333;
            composes:bigBox from './common.css' 
        }
        ```

- 如何支持less/sass/stylus

  - 安装

    - ```shell
      npm install less-loader --save--dev
      npm install sass-loader node-sass --save--dev
      ```

  - 配置

    - ```javascript
             rules: [
               {
                 test: /\.css$/,
                 use: [ 
                     {
                         loader:'style-loader',
                         options:{
                             // insertInto:'#app',
                             // singleton:true,
                             // transform:'./css.transform.js'
                         }
                     },{
                         loader:'css-loader',
                         options:{
                             modules: true,
                             localIdentName:'[path][name]__[local]--[hash:base64:5]'
                         }
                     },{
                         loader:'less-loader',
                     }
                  ]
               }
             ]
         ```
    ​

- 如何支持PostCss

  - 什么是PostCss?

    - A tool transformimg css with js
    - 转化css的工具
    - 安装
      - ` cnpm install postcss postcss-loader -D`
    - 配置
      - ​

  - PostCss最有名的插件

    - Autoprefixer

      - 增加浏览器前缀

      - ```css
        a{
            display:flex;
        }

        a{
            display:-webkit-box;
            display:-webkit-flex;
            display:-ms-flexbxox;
            display:flex; 
        }
        ```

      - 安装

        - `cnpm install autoprefixer -D`

      - 使用

        - ```javascript
                 rules:[
                     {
                         test:'/\.css$/',
                         use:ExtractPlugin({
                             fallback:{
                                 loader:'style-loader'
                             },
                             use:[
                                 {
                                 loader:'css-loader'
                             },
                             {
                                 loader:'postcss-loader',
                                 options:{
                                     ident:'postcss',  //指明下面的插件是给postcss使用的
                                     plugins:[
                                         require('autoprefixer')(), //调用
                                         require('postcss-cssnext')() //postcss-next中包含了autoprefixer
                                     ]
                                 }
                             },
                             {
                                 loader:'less-loader'
                             }]
                         })
                     }
                 ]
             ```
          ```

          ```

      - 注意

        - 对于浏览器对css3支持上面，我们需要制定浏览器browerlist这个配置项

        - css-loader也需要制定browserlist，我们统一设置

          - 两种方式一种在package.json中设置

          - 一种在项目根目录下新建.browserlistrc文件，并在其中设置

          - ```javascript
             "browserlist":[
                ">= 1%",
                "last 2 versions"
              ]
             ```
            ```

            ```

    - CSS-nano

      - 帮助优化和压缩css
      - css-loader中minimize使用的就是css-nano来压缩css代码
      - 安装
        - ` cnpm install cssnano -D`

    - CSS-next

      - 支持未来的css语法
      - 安装
        - `cnpm install postcss-cssnext -D`
        - css 变量
        - 自定义选择器
        - calc()计算函数

- 提取CSS代码

  - 为什么要提取CSS代码

    - CSS代码也可能是公共代码，为了减少代码冗余，提高用户访问速度

  - 如何提取

    两种方式

    - extract-loader

      - 不介绍

    - ExtractTextWebpackPlugin（主流方式）

      - 安装

        - ` cnpm install extract-text-webpack-plugin -D`

      - 配置

      - 打包好的css文件需要使用link标签引入到html中

        - ```javascript
          //引入
          var ExtractPlugin = require('extract-text-webpack-plugin');

          //插件
          plugins:[
              new ExtractPlugin({
                   filename:'[name].min.css',    //提取出来的css文件名
                   allChunks:false
                   //默认是false,如果是true会将所有import进来的css文件打包到一个css文件中
                  //如果是false则只会提取初次加载的css,那些异步加载的css文件不会被提取
              })
           ]

          //文件处理
                  rules:[
                      {
                          test:'/\.css$/',
                          use:ExtractPlugin({
                              fallback:{   //如果不提取出来单独的文件应该怎么处理
                                  loader:'style-loader'
                              },
                              use:['css-loader','less-loader']
                          })
                      }
                  ]
          ```


### webpack-----Three-shaking

- 解释

  - 摇树（把枯叶子摇下来）
  - 对项目中的js和css从未使用过的代码使用three shaking在打包的时候将其去掉

- 使用场景

  - 常规优化
  - 引入第三方库的某一个功能

- 如何实现

  - 在webpack2之后，就会将一些没有使用到的代码进行标记
  - 在打包的时候借助插件将没有用的代码去掉

- JS three Shaking

  - - 本地多余代码

      - 标记

        - harmony export模块方式
        - unused harmony export未使用的模块化

      - 配置

        - ```javascript
          var webpakc = require('webpack')

           plugins:[
                  new ExtractPlugin({
                      filename:'[name].min.css',
                  }),
                  new Webpack.optimize.UglifyJsPlugin()
              ]
          ```

    - 引入第三方库但未使用库里的所有方法

      - 以`lodash`为例

        - 查看loash源码查看他是不是用模块化的方式来编写的

        - 如果不是，打包的时候很可能无法优化

        - 就需要寻找其他的办法

          - 安装`loash-es`

          - 再次尝试打包还是不能将多余的代码去掉

          - 再去使用其他方法使用`babel-plugin-loadsh`

            - 配置

              - ```javascript
                    {
                                test:'/\.js$/',
                                use:{
                                    loader:'babel-loader',
                                    options:{
                                        presets:['env'],
                                        plugins:['loadsh']
                                    }
                                }
                            }
                ```

- CSS three Shaking

  - 需要借助 purify css

    - 安装

      - `cnpm  install purifycss-webpack glob-all --save-dev`

    - 配置

      - paths:路径

        - 会检查这个路径上的css

        - glob.sync([]) 处理多路径

        - 栗子：

          - ```javascript
            var PurifyCSS = require('purifycss-webpack')
            var glob = require('glob-all')
            plugins:[
                    new ExtractPlugin({
                        filename:'[name].min.css',
                    }),
                //写在ExtractPlugin插件之后
                    new PurifyCSS({
                        paths:glob.sync([
                            path.resolve(__dirname,"./*.html"),
                            path.resolve(__dirname,"./src/*.js")
                        ])
                    }),
                    new Webpack.optimize.UglifyJsPlugin()
                ]
            ```

### 图片处理

- 图片处理遇到的场景

  - css中引入图片
    - file-loader
  - 合并图片成雪碧图
    - postcss-sprites
  - 压缩图片
    - img-loader
  - 小图片的话使用base64编码
    - url-loader

- 安装

  - ```shell
    npm install file-loader --save-dev
    npm install url-loader --save-dev
    npm install img-loader --save-dev
    npm install postcss-sprites --save-dev
    ```

- 配置

  - file-loader

    - ```javascript
         {
             loader:'file-loader',
              options:{
                    publicPath:'',
                    outputPath:'dist/',   //输出文件的路径
                    useRelativePath:true  //是否使用相对响度路径
                }
          }
         ```
      ```

      ```

  - url-loader

    - ```javascript
        {
             loader:'url-loader',
              options:{
                    name:'[name][hash:5]min.[ext]', //输出文件名称
                    limit:1000,   //小于1000KB的图片就会被转成base64
                    publicPath:'',
                    outputPath:'dist/',   //输出文件的路径
                    useRelativePath:true  //是否使用相对响度路径
                }
          }
        ```
      ```

      ```

  - img-loader

    - ```javascript
      {
          	loader:'img-loader',
              options:{
                  pngquant:{
                      quality:80  //规定压缩后图片的质量
                  }
              }
      }
      ```

  - postcss-sprites

    - ```javascript
         {
               loader:'postcss-loader',
               options:{
                   ident:'postcss',
                   plugins:[
                       require('autoprefixer')(),
                       require('postcss-cssnext')(),
                       require('postcss-sprites')({
                           //生成精灵图的路径
                           spritePath:'dist/assets/imgs/sprites'，
                           retina:true  
                           //针对苹果产品，使用的retina屏幕，设计师一般会做两倍大小的图，我们需要设置如上
                       })
                   ]
               }
         ```
        }

      //同时修改图片名称告诉postcss哪些是retina屏幕需要用到的图片
      // 图片名@2x
      //css也做相应应的修改
      	//图片大小改为两倍图片大小的一半
      	//引用的图片名称也修改为两倍图片大小的图片名称
      ```

      ```

### 字体文件处理

- 字体下载到本地

  - css配置

    ```css
    @fomt-face{
        font-family:'FontAwsome';
        src:url('../assets/fonts/fontawsome-webfont.eot?#iefix&v=4.7.0');
        src:url('../assets/fonts/fontawsome-webfont.woff2?v=4.7.0');
        src:url('../assets/fonts/fontawsome-webfont.woff?v=4.7.0'); 
        src:url('../assets/fonts/fontawsome-webfont.ttf?v=4.7.0'); 
        src:url('../assets/fonts/fontawsome-webfont.svg?v=4.7.0'); 
        font-weight:normal;
        font-style:normal;
    }

    .fa {
        display:inline-block;
        font:normal normal normal 14px/1 FontAwesome;
        font-size:inherit;
        text-rendering:auto;
        -webkit-font-smothing:antialiased;
        -moz-osx-font-smothing:grayscale;   
    }

    .fa-floader:before{
        content:'\f07b';
    }
    ```


- 配置

  - ```javascript
    {
        test:/.\(eot|woff2?|ttf|svg)$/;
        use:[
            {
                loader:'url-loader',
                options:{
                  name:'[name][hash:5]min.[ext]', //输出文件名称
                  limit:5000,  
                  publicPath:'',
                  outputPath:'dist/',   //输出文件的路径
                  useRelativePath:true  //是否使用相对响度路径
               }
            }
        ]
    }
    ```

### 第三方库js处理

- 场景

  - 第三方库在远程的CDN上，需要在页面上引用链接
  - 自己项目目录下有一个第三方库

- 处理方式

  - 借助插件webpack.providePlugin

    - 配置参数(json)

      - key-value
      - key：变量的名称可以使用
      - value : 模块的名称==require或者import

    - 栗子

      - 来自于npm下载的

      - ```javascript
        plugins:[
            new webpack.providePugin({
                $:'jquery'
            })
        ]
        ```

      - 自己目录下的第三方库

      - ```javascript
        resolve:{
            jquery$:path.resolve(__dirname,'src/libs/jquery.min.js')
        }
        plugins:[
            new webpack.providePlugin({
                $:'jquery$'
            })
        ]
        ```

      - ​

  - 借助loader

    - imports-loader

    - 安装

      - `cnpm install imports-loader --save-dev`

    - 配置

      - 从npm上download下来的

      - ```javascript

        {
            test:path.resolve(__dirname,'src/app.js'),
                use:[
                    {
                        loader:'imports-loader',
                        options:'jquery'
                    }
                ]
        }
        ```

      - 项目目录下的第三方库

      - ```javascript
        resolve:{
            jquery$:path.resolve(__dirname,'src/libs/jquery.min.js')
        }

        {
            test:path.resolve(__dirname,'src/app.js'),
                use:[
                    {
                        loader:'imports-loader',
                        options:'jquery$'
                    }
                ]
        }
        ```

      - ​

  - window对象上加入

    - 远程cdn上的jq
    - 第一步：在html上引入jq
    - 第二步在js中使用jq的$

### html in webpack

- 如何在项目中生成html

  - 插件HtmlWebpackPlugin

    - 配置options
      - template：模板文件，（对应的模板类型要配置相应的解析loader）
      - filename：生成html的文件名
      - minify: 是否压缩
      - chunks：指定有那几个entry chunk需要加载到html中，如果不指定默认是所有的chunk都会被加入到html中
      - inject:是不是需要将生成的css和js插入到html中

  - 安装

    - `cnpm install HtmlWebpackPlugin --save -dev`

  - 栗子

    - ```javascript
      var HtmlWebpackPlugin = require('HtmlWebpackPlugin')
      plugins:[
          {
              new HtmlWebpackPlugin({
              	filename:'index.html',
              	template:'./index.html',
              	chunks:['app']
              	inject:false,
              	minify:{
              		collapseWhitespace:true  //压缩空格
          		}
         		 })
          }
      ]
      ```

- html中如何使用图片

  - 使用require来寻找图片

    - `<img src="requie('相对路径')" />`

  - 借助html-loader指定html中哪些参数是要交给webpack处理的

  - html-loader

    - 配置options

      - attrs:[]  每一项是一个规则
      - webpack根据这些规则来处理响应的文件

    - 栗子

      - ```javascript
        {
            test:/\.html$/,
            use:[
                    loader:'html-loader',
                    options:{
                    	attrs:['img:src','img:data-src']
                    }
                ]
        }

        <img src='相对路径' data-src='相对路径' />
            
        //路径问题
        //css中引用图片和html引入图片路径不一致
        //在图片处理的loader中配置的
            outputPath:'dist/'
        	publicPath:''
        	useRelativePath:true
        //处理
        	 outputPath:'dist/'
        	 publicPath:'assets/src/imgs/'
            
        ```

- html打包优化

  - 公共代码需要http请求引入，可以优化。打包的时候将公共代码以script标签的形式插入html中

    - 提前载入webpack加载代码

      - 借助插件html-webpack-inline-chunk-plugin

      - 安装

        - `cnpm install html-webpack-inline-chunk-plugin --save-dev `

      - 配置

        - ```javascript
          var HtmlInLinkChunkPlugin = require('html-webpack-inline-chunk-plugin')
          plugins:[
              new HtmlInLinkChunkPlugin({
                  inlineChunks:['mainfest'] //chunk名称
              })
          ]
          ```

# webpack环境配置

## 搭建本地开发环境



- 为什么要搭建本地开发环境
- 因为正常我们的项目是运行在web服务器上的，而不是我们本地调试的这种方式

三种种实现方式

- webpack - watch

  - `webpack -w -process --display-reasons --color`

    - 显示进入
    - 颜色区分
    - 一旦代码有改动，会自动重新打包

  - 每次打包我们想要先清除原来的可以借助一个插件clean-webpack-plugin

    - 使用

    - ```javascript
      var CleanWebpackPlugin = require('clean-webpack-plugin')
      plugins:[
          new CleanWebpackPlugin（['dist']）
      ]
      ```

- webpack-dev-server

  - 功能介绍

    - live reloading  代码改动刷新浏览器
    - 不能用来打包文件，打包的结果是存在webpack内存中
    - 路径重定向
    - 浏览器中显示错误
    - 接口代理
    - 模块热更新  不刷新代码的情况下刷新页面

  - devServer

    - inline
    - contentBase
    - port
    - historyApiFallBack
    - https
    - proxy
    - hot
    - openpage
    - lazy:只有访问到的资源才打包与编译（在多页面应用中比较有用）
    - overlay:在浏览器编译错误的提示

  - 安装

    - `cnpm install webpack-dev-server --save-dev`

  - 配置

    - ```javascript
      //package.json文件中
      "scripts":{
          "server":"webpack-dev-server --open"
      }
      ```




      //webpack.config.js文件中

      //开启sourceMap在webpack中对代码进行调试
      //将生成的代码和原来的代码做一个映射
      // JS SourceMap
      	//配置devtool
      		//devtool有7个参数
      		//开发环境
      			//eval
                   //eval-source-map
      			//cheap-eval-source-map
      			//cheap-module-source-map
      			//每一个的运算时间不一样
      		//生产环境
      			//source-map
      			//hidden-source-map
      			//nosource-source-map
    
      //开发的时候的需要选择：cheap-module-source-map
      devtool：'eval',
      // Css SourceMap
      	//除了设置sourceMap之外对于样式还需要对对应的loader的sourceMap
      	//css-loader.option.sourceMap
      	//less-loader.option.sourceMap
      	//sass-loader.option.sourceMap
       module:{
           rules:[
               {
                   test:/\.css$/,
                   use:[
                       {
                           loader:'style-loader',
                           options:{
                               sourceMap:true
                           }
                       },{
                           loader:'css-loader',
                           options:{
                               sourceMap:true
                           }
                       },{
                           loader:'sass-loader',
                           options:{
                               sourceMap:true
                           }
                       }
                      
                   ]
               }
           ]
      },

​          
      devServer：{
          port:9001,
          inline:true ,//默认值是true，如果为false则在页面顶部会有一条显示打包进度的信息显示
          //hostoryApiFallBack：true, //简单使用，任何路径都会被定位到index.html
          historyApiFallBack{
              //提供给html5 historyAPI使用的
              rewrites:[
                  {
                      form:'/pages/a',
                      to:'/pages/a.html'
                  },
                  {
                      from:/^\/([a-zA-Z0-9]+\/?)([a-zA-Z0-9]+)/,
                      to:function(context){
                          return "/"+context.match[1]+context.match[2]+".html";
                      }
                  }
              ]
          },
          //代理远程接口请求（借助的是第三方插件http-proxy-middleware）
              //参数options
              // target：目标地址
              //changeOrigin：true必须设置，否则请求不成功
              //header：增加http请求的头
              //logLevel:在命令控制台中打印出来设置的代理信息
              //pathRewrite: 请求的地址太长不想写那么长
              		pathRewrite:{
                          '/comments':'/api/comments'
                      }
              //请求地址：http://m.weibo.cn/api/comments/show?id=435435943534959&page=1
          	//js文件中的请求
      		// $.get("/api/comments/show",{
          	//	id:435435943534959,
         		//	 page:1
      		//},function(data){
         		//	 console.log(data);
      		//})
              proxy:{
                  '/api':{
                      target:'http://m.weibo.cn',
                      changeOrigin:true,
                      logLevel:'debug',
                      headers:{
                              "Cookies": sdsjdbjs
                              //...还有其他参数
                      }
                  }
              },
                  
              //模块热更新
                  //优点：
                  	//节省调试时间
                  	//样式调试更快
                  	//保持应用数据状态
                 //设置hot属性
                 //借助插件
                  	//webpack.HotModuleReplacementPlugin
                  	//webpack.NamedModulesPlugin  查看模块的具体名称
                  //代码中，
                  	//通过module.hot接口
                  	// module.hot.accept 接口
                  	//css中代码的热更新借助于style-loader就会自动帮助你热更新
                  	//js中的代码热更新有时候需要自己写代码，框架中有一些loader会帮助对代码进行热更新
                  //需要设置
                  //plugins:[
          		//{	
             		 	//new webpack.HotModuleReplacementPlugin(),
              	  	//new webpack.NamedModulesPlugin()
          		//}
      		   //]
                hot:true,
                hotonly:true
      }
    
      ```

```
  //使用命令
  npm run server
```

- Express +webpack-dev-middleware

## Eslint检查语法

- 开发时候我们需要统一开发代码风格

- Eslint使得devServer支持每一次我们代码提交时进行书写规范的检查，代码不符合规范不通过编译

- 安装

  - eslint-loader

    - 配置options
      - failOnWarning：true 当出现规范警告时编译不通过
      - failOnWarning：true 当出现规范错误时编译不通过
      - formatter:设置第三方错误输出方式
      - outputReport：输出代码检查报告

  - eslint-plugin-html(当js代码以script标签的形式内嵌在html中时也需要规范检查时)

  - eslint-friendly-formatter  错误信息输出的格式

  - devServer.overlay:在浏览器显示错误

  - 安装栗子

    - ```shel
      cnpm install eslint-loader eslint-plugin-html eslint-friendly-formatter --save-dev
      ```

    ​

    ​

- 配置

  - 在webpack.config中配置

    - ```javascript
      {
          test:/\.js$/,
          include:[path.reslove(__dirname,'src')],
          exclude:[path.reslove(__dirname,'lib')],
          use:[
             	 {
                  loader:'babel-loader',
                  options:{
                       presets:['env']
                   }
               },
              {	
                  loader:'eslint-loader',
                  options:{
                      formatter:require(' eslint-friendly-formatter')
                  }      
              }]
      }
      ```

  - 配置一个.eslintrc.*的文件

    - 写一些代码规范

    - ```javascript
      //新建文件.eslintrc.js文件
      module.exports = {
          root:true,
          extends:'standard',
          plugins:[],
          globals:{
              //全局变量
            "$":true  
          },
          env：{
          	//环境
          	browser:true,
          	node:true
      	}，
          rules:{
              //自定义的需要覆盖standard标准的规范规则
              //第一个值表示错误的严重性
              indent:["error",4]，
              "eol-last":["error",none]
              
          }
      }
      ```

    - ​

  - Javascript Standard Style（代码规范）

    - 遵循这种规范的话需要安装以下插件
      - eslint-config-satndard
      - eslint-plugin-promise
      - eslint-plugin -satndard
      - eslint-plugin-import
      - eslint-plugin-node
      - 安装`cnpm install eslint-config-satndard eslint-plugin-promise eslint-plugin-standard eslint-plugin-import eslint-plugin-node --save-dev` 

## 区分开发环境和生产环境

- 开发环境

  - 模块热更新
  - sourceMap
  - 代码规范检查
  - 接口代理

- 生产环境

  - 提取公用代码
  - 压缩混淆
  - 文件压缩或者base64
  - three shaking 去除多余代码

- 共同点

  - 同样的入口
  - 同样的代码处理（什么类型的文件使用什么loader处理）
  - 同样的解析配置

- 如何区分

  - webpack-merge
    - webpack.dev.conf.js
    - webpack.prod.conf.js
    - webpack.common.conf.js

- 栗子

  - 安装`webpack-merge`

    - `cnpm install webpack-merge --save-dev`

  - 修改package.json做配置

    - ```javascript
      "scripts":{
          "build":"webpack --env production --config build/webpack.dev.conf.js",
          "server":"webpack-dev-server --env development --open --config build/webpack.common.conf.js"
          
      }
      ```

  - 在项目根目录下新建build文件夹来存放所有的webpack配置

    - webpack.common.conf.js

      - ```javascript
        var productionConfig = require('./webpack.prod.conf.js')
        var developmentConfig = require('./webpack.dev.conf.js')

        var merge = require('webpack-merge')
        var ExtractTextWebpackPlugin = require('extract-text-webpack-plugin')
        var path = require('path')
        var webpack = require('webpack')
        var htmlWebpackPlugin = require('html-webpack-plugin')
        ```


        const generateConfig = env=> {
            
            //引入文件
            const extractLess = new ExtractTextWebpackPlugin({
                filename:'css/[name]-bundle-[hash:5].css'
            })
            
            //提取loader
            
            //处理.js文件的loader
            //关于babel的配置可以放在.babelrc文件中
            const scriptLoader = [loader:'babel-loader'].contact(env==='production?								[]:[{
                                            loader:'eslint-loader',    
                                            options:{
                                                  formatter:require('eslint-friendly-formatter')              
                                             }
                                        }]
                                  )
             
            //处理样式相关的文件
            //开发环境使用style-loader打包,生产环境需要借助插件ExtractTextPlugin
              const cssLoaders = [
                 					{
                      					loader:'css-loader',
                                          options:{
                                            importLoaders:2,
                                            sourceMap:env==='development'
                                         }
                                    },{
                                        loader:'postcss-loader',
                                        options:{
                                            ident:'css',
                                            sourceMap:env==='development',
                                            plugins:[
                                                require('postcss-cssnext')()
                                            ].contact(env==="development"?
                                            [require('postcss-sprites')({       		
                                                spritePath:'dist/assets/imgs/sprites',
                                                   retina:true                         
                                                })]:[])
                                        }
                                    },{
                                     	loader:'less-loader',
                                        options:{
                                            sourceMap:env==='development'
                                        }
                                    }
                                  ]
               const styleloader = env==="production"?extractLess({
                                      fallback:'style-loader',
                   					 use:cssLoaders
              					 }):['style-loader'].contact(cssLoaders);
            
            //配置图片，字体等文件的laoder
        	const fileLoader = env==='development'?[
            	{
                	loader:'file-loader',
                    options:{
                        name:'[name]-[hash:5].[ext]',
                        output:'assets/imgs/'
                    }
            	}
            ]:[
                {
                    loader:'url-loader',
                    options:{
                        name:'[name]-[hash:5].[ext]',
                        limit:1000,
                        outputPath:'assets/imgs/'
                    }
                }
            ]

            // 返回一个配置对象
            return {
           			entry：{
                		app:'src/app.js'
        			}，
            		output:{
           		     	path:path.resolve(__dirname,'dist'),
                         publicPath:'/',
                         filename:'js/[name]-bundle-[hash:5].js'
            		},
                     resolve:{
                         alias:{
                             jquery$:path.resolve(__dirname,'../src/libs/jquery.min.js')
                         }  
                     },
                     module:{
                         rules:[
                             {
                             	test:/\.js$/,
                                 include:[path.resolve(__dirname,'src')],
                                 exclude:[path.resolve(__dirname,'src/libs/')],
                                 use:scriptLoader
                         	 },
                             {	
                                 test:/\.css$/,
                                 use:styleLoader  
                             },
                             {
                                 test:/\.(png|jpg|jpeg|gif)$/,
                                 use:fileLoader.contact(
                                     env==="production"?{
                                        loader:'img-loader',
                                         options:{
                                             pngquant:{
                                                 quanity:80
                                             }
                                         }
                                     }:[]
                                 )
                             },
                             {
                                 test:/\.(eot|woff2?|ttf|svg)$/,
                                 use:fileLoader
                             }
                         ]       
                      },
                      plugins:[
                          extractLess,
                          new htmlWebpackPlugin({
                              filename:'index.html',
                              template:'./index.html',
                              minify:{
                                  collapseWhitespace:true
                              }
                          }),
                          new webpack.ProvidePlugin({
                              $:'jquery'
                          })
                       ]
        	}
        }
    
        module.exports = env=>{
            let config = env ==='production'? productionConfig :developmentConfig
            return merge(generate(env),config);
        }
    
        ```
    
    - webpack.prod.conf.js
    
      - ```javascript
        var webpack = require('webpack')
        var PurifyWebpack = require('purifycss-webpack')
        var  HtmlInLinkChunkPlugin = require('html-webpack-inline-chunk-plugin')
        var CleanWebpackPlugin = require('clean-webpack-plugin')
    
        var path = require('path')
        var glob = require('glob-all')
        module.exports = {
            plugins:[
                new PurifyWebpack({
                    path:glob.sync([
                        './*.html',
                        './src/*.js'
                    ])
                }),
                new webpack.optimize.CommonsChunkPlugin({
                    name:'mainfest'
                }),
                new HtmlInLinkChunkPlugin({
                    inlineChunks：['mainfest']
                }),
                new  webpack.optimize.UglifyJsPlugin(),
                new CleanWebpackPlugin(["dist"])
            ]
        }
        ```
    
      - ​
    
    - webpack.dev.conf.js
    
      - ```javascript
    
        const webpack = require('webpack')
        module.exports = {
          
                devtool:'cheap-module-source-map',
                devServer:{
                	port:9001,
                	overlay:true,
                	proxy:{
                		'/api':{
                        target:'http://m.weibo.cn',
                        changeOrigin:true,
                        logLevel:'debug',
                        headers:{
                                "Cookies": sdsjdbjs
                                //...还有其他参数
                        }
                    }
               		 },
                	hot:
                	hotonly:true,
                	historyApiFallBack:{
                		 rewrites:[
                   					 {
                       					 form:'/pages/a',
                       					 to:'/pages/a.html'
                    				},
                    				{
                        				from:/^\/([a-zA-Z0-9]+\/?)([a-zA-Z0-9]+)/,
                       					 to:function(context){
                            			return "/"+context.match[1]+context.match[2]+".html";
                        			}
                    		}]
                	}
                },
                    plugins:[
                        new webpack.HotModuleReplacementPlugin(),
                        new webpack.NamedModulesPlugin()
                    ]
            
        }
        ```

## webpack 实战

- 分析打包结果

  - 官方提供的打包分析工具,产生一个json文件，在http://webpack.github.io/analyse/可以上传生成的文件进行可视化的分析

    - mac中
      - `webpack  --profile --json >stats.json`
    - windows中
      - `webpack --profile --json |Out-file'stats.json' -Encoding OEM`

  - 社区提供的打包分析工具

    - 插件 BundleAnalyzerPlugin

      - 安装

        - `cnpm install webpack-bundle-analyzer --save-dev`

      - 配置

        - ```javascript
          var BundleAnalyzerPlugin = require('webpack-bundle-analyzer').webpack-bundle-analyzer

          plugins:[
              new BundleAnalyzerPlugin()
          ]
          ```

        - webpack打包之后自动从浏览器弹出一个分析页面

    - 命令行 webpack-bundle-analyzer stats.json  分析stats.json

- 优化打包速度

  - 影响打包速度的因素

    - 文件多？
    - 依赖多？
    - 页面多？
    - 使用loader是否合适？
    - 使用操作耗时的编译方式？

  - 如何优化

    - 区分第三方代码(vender)和业务代码(app)

      - 将第三方代码的打包和业务代码的打包区分开

        - 借助插件DllPlugin打包第三方代码生成一个map(映射关系),
        - 借助插件DllReferencePlugin打包业务代码引用映射

      - 例子

        - ```javascript
          // 新建dll的配置文件 webpack.dll.config.js

          const path = require('path')
          const webpack = require('webpack')
          module.exports = {
              entry:{
                 vue：["vue","vue-router"] ,
                  ui:["element-ui"]
              }，
              output:{
              	path：path.join(__dirname,'../src/dll/'),
              	filename:'[name].dll.js',
              	library:'[name]'   //定义引用的地方如何引用
          	},
              plugins:[
                  new webpack.DllPlugin({
                      path：path.join(__dirname,'../src/dll/','[name]-mainfest.json'),
                      //打包文件输出目录,json文件是map的输出文件
                      name:[name]
                  }),
                  new webpack.optimize.UglifyJsPlugin()
              ]
          }	

          webpack build/webpack.dll.conf.js

              webpack.prod.js
                plugins:[
                    //引用
                    new webpack.DllReferencePlugin({
                        mainfest:require('../src/dll/ui-mainfest.json')
                    }),
                    new webpack.DllReferencePlugin({
                        mainfest:require('../src/dll/vue-mainfest.json')
                    })
                ]
          ```

    - 上线前压缩和混淆代码（UglyfiJsPlugin）

      - 设置属性parallel:并行处理

      - ```javascript
         new UglyfiJsPlugin({
                uglyfiOptions:{
                    compress:{
                        warning:false
                    }
                },
                sourceMap:config.build.productionSourceMap,
                parallel:true,
                cache:true
            })
         ```
        ```

        ```

    - HappyPack插件

      - 给loader使用，串行处理变成并行的

      - 线程池

      - 安装

      - 配置

        - `cnpm install happypakck --save-dev`

        - ```javascript
          plugins:[
              new HappyPack({
                  id:'vue',
                  loaders:[
                      {
                          loader:'vue-loader',
                          option: require('./vue-loader.conf')
                      }]
              })
          ]
          ```



          //使用
          {
           
              test:/\.vue/,
              loader:'happypack/loader?id=vue',
              options:vueLoaderConfig
          }
          ```
    
        - ​
    
    - Babel-loader
    
      - 开启缓存 cacheDirectory
      - include：规定babel-loader打包范围
      - exclude：规定babel-loader打包范围
    
    - 其他
    
      - 减少resolve
        - 去除sourceMap
        - cache-loader
        - 升级node和webpack

- 长缓存优化
  - 什么是长缓存优化？
    - 用户发起请求，服务器对请求的资源的头部进行设置来告诉请求的浏览器有些资源是一段时间内都不会更新的，不用每次都请求
    - 每一个资源带有自己的版本号，版本号没有发生变化的时候可以不需要更新
  - webpack如何实长缓存
    - 场景 app+vendor

      - app改变，vendor不变化

        - app和vendor分开分别打包

        - hash换成chunkhash

        - 提取webpack runtime（webpack runtime的改变也会影响vendor）

        - 配置

          - ```javascript
            const path = require('path')
            const webpack = require('webpack')

            module.exports ={
                entry:{
                    main:'./src/foo',
                    vendor:['react']
                },
                output:{
                    path:path.resolve(__dirname,'dist'),
                    filename:[name].[chunkhash:8].js
                },
                plugins:[
                    new webpack.optimize.CommonsChunkPlugin({
                        name:'vendor'
                    }),
                    new webpack.optimize.CommonsChunkPlugin({
                        name:'mainfest'
                    })
                ]
            } 
            ```

    - 场景 引入新模块，模块顺序变化，vendor hash变化

      - 因为webpack在打包的时候会为每一个chunk随机分配一个id，一旦引入的顺序变化对应的id也会变化，那么对应chunkhash也会变化

      - 解决办法:使用NamedChunksPlugin和NamedModulesPlugin

      - ```javascript
        plugins:[
            new webpack.NamedChunksPlugin(),
            new webpack.NamedModulesPlugin()
        ]
        ```

    - 场景 动态引入模块时，vendor hash变化

      - 这个场景最新的webpack版本已经避免了
- webpack多页面应用

  - vue等框架通常是开发单页面应用程序

  - 多页面应用

    - 多个entry
    - 多页面html
    - 每个页面不同的chunk（每一个页面引入不同的js和css）
    - 每个页面有不同参数

  - 解决方案

    - 多页面多配置(每一个页面一个配置)

      - 优点

        - 可以使用parallel-webpack来提高打包的速度
        - 配置更加独立与灵活

      - 缺点

        - 不能多页面之间共享代码

      - 配置：

        - 安装

          - `cnpm install webpack-merge webpack  --save--dev`
          - `cnpm install html-webpack-plugin --save--dev`
          - ` cnpm install clean-webpack-plugin --save--dev`
          - `cnpm install extract-text-webpack-plugin --save--dev`
          - `cnpm install webpack-merge --save-dev`
          - `cnpm install style-loader --save-dev`
          - `cnpm install css-loader --save-dev`

        - ```javascript
          //webpack.config.js
          const merge = require('webpack-merge')
          const webpack = require('webpack')
          const htmlWebpackPlugin = require('html-webpack-plugin')
          const CleanWebpack = require('clean-webpack-plugin')
          const ExtractTextWebpackPlugin = requrie('extract-text-webpack-plugin')

          const path = reuqire('path')

          const baseConfig = {
              entry:{
                  react:['react']
              },
              output:{
                  path:path.resolve(__dirname,'dist'),
                  filename:'js/[name].[chunkhash].js'
              },
              module:{
                  rules:[
                      {
                          test:/\.css/,
                          use:ExtractTextWebpackPlugin.extract({
                             fallback:'style-loader',
                              use:'css-loader'
                          })
                      }
                  ]
              },
              plugins:[
                  new CleanWebpack(path.resolve(__dirname,'dist')),
                  new webpack.optimize.CommobChunkPlugin({
                      name:'react',
                      minChunks:Inifinity
                  }),
                  new ExtractTextWebpackPlugin({
                      filename:'css/[name].[hash].css'
                  })
              ]
          }


          //pages配置函数
          const generatePage = function({
              title='',
              entry='',
              template='./src/index.html',
              name='',
              chunks=[]
          }=>{}){
              return {
                  entry,
                  plugins:[
                      new HtmlWebpackPlugin({
                          chunks,
                          template,
                          filename:name+'.html'
                      })
                  ]
              }
          }
          const pages = [
              generatePage({
              	title：'pageA',
                  entry：{
                  	a:'./src/pages/a'
              	},
                   name:'a',
                   chunks:['react','a']
          	}),
               generatePage({
              	title：'pageB',
                  entry：{
                  	b:'./src/pages/b'
              	},
                   name:'b',
                   chunks:['react','b']
          	}),
               generatePage({
              	title：'pageC',
                  entry：{
                  	c:'./src/pages/c'
              	},
                   name:'a',
                   chunks:['react','c']
          	})  
          ]

          //基础配置和pages配置进行merge
          module.exports = pages.map(page => merge(baseConfig,page))

          ```

          ​

    - 多页面单配置

      - 优点

        - 可以共享各个entry之间的公共代码

      - 缺点

        - 打包速度很慢
        - 输出内容复杂

      - 配置

        - ```javascript
          module.exports = merge(baseConfig.contact(pages))
          ```

# Webpack和Vue

- Vue官方脚手架
  - 安装
    - `cnpm install vue-cli -g` 
- 项目模板
  - ​
- 配置文件

# webpack 面试常见问题

- 概念
  - 什么是webpack?和gulp和gunt有什么不同？
    - webpack是一个模块打包器，他可以递归的打包项目中所有的模块，最后生成几个打包后的文件。
    - 他和其他工具最大的不同在于他支持代码分割，模块化（ESM，AMD，CommonJs）,全局分析
  - 什么是bundle?什么是chunk?什么是module?
    - bundle是webpack打包出来的文件，chunk是webpack进行模块依赖分析的时候代码分割出来的代码块，module是开发中的单个模块
  - 什么是loader?什么是plugin?
    - Loaders是告诉webpack如何转化处理某一类型的文件，并且引入到打包的文件中
    - Plugin是用来自定义webpack打包过程的方式，一个插件含有apply方法的一个对象，通过这个方法可以参与到webpack整个打包的各个流程中
- 配置
  - 如何自动生成webpack的配置？
    - 借助webpack-cli，vue-cli等配置脚手架的东西
- 开发
  - webpack的webpack-dev-serve=(express+hotmiddleware)和普通的http服务器如nginx有什么区别?
    - webpack-dev-server使用内存来存储webpack开发环境下的打包文件，并且可以使用模块热更新，他比传统的http服务对开发更加简单高效
  - 什么是模块热更新？
    - 模块热更新是webpack的一个功能，他可以使得代码修改过后不用刷新浏览器就可以更新，是高级的自动刷新浏览器
- 优化
  - 什么是长缓存?在webpack中如何做到长缓存？
    - 浏览器在用户访问页面的时候，为了加快加载速度，会对用户访问的静态资源进行存储，但是代码每一次升级或者更新，都需要浏览器去下载最新的代码，最方便和最新的方式引入新的 文件名称。
    - 在webpack中可以在output中指定输出的文件指定的chunkhash,并且分离更新的代码和框架代码。
    - 通过NamedModulesPlugin或者是NamedModuleIdsPlugin使再次打包文件名不变
  - 什么是Three-Shaking?CSS可以three Shaking?
    - Tree-Shaking是在打包中去除那些引入了但是在代码中没有用到的死代码。在webpack中Tree-shaking通过uglifyJSPlugin来Tree-ShakingJS,css需要使用Purify-css







