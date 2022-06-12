[Toc]

# 基础
## 前言
### 为什么需要打包工具
开发时，会使用React、Vue等框架，ES6模块化语法，Less/Sass等css预处理器等语法进行开发。

这样的代码要想在浏览器中运行必须经过编译成浏览器能识别的JS、CSS等语法才能运行。

所以需要打包工具做完这些事情。

除此之外，打包工具还能压缩代码、做兼容性处理、提升代码性能等。

### 有哪些打包工具
+ Grunt
+ Gulp
+ Parcel
+ Webpack
+ Rollup
+ Vite
+ ...

目前主流使用Webpack。

## 基本使用
```webpack```**是一个静态资源打包工具。**
它会以一个或多个文件作为打包的入口，把整个项目所有文件编译组合成一个或多个文件输出出去。

输出的文件就是编译好的文件，就可以在浏览器端运行了。`webpack`输出的文件叫做`bundle`。

### 功能介绍
`webpack`本身功能有限：
+ 开发模式：仅能编译JS中的`ES Module`语法；
+ 生产模式：能编译JS中的`ES Module`语法，还能压缩JS代码。

### 开始使用
#### 1. 资源目录
#### 2. 创建文件
#### 3. 下载依赖

打开终端，切换到项目根目录。
+ 初始化`package.json`
```shell
npm init -y
```
此时会乘胜一个基础的`package.json`文件。

需要注意的是`package.json`中`name`的字段不能叫做`webpack`，否则下一步会报错。
+ 下载依赖
```shell
npm i webpack webpack-cli -D
```

#### 4. 启用weipack
+ 开发模式
```shell
npx webpack ./src/main.js --mode=development
```
+ 生产模式
```shell
npx webpack ./src/main.js --mode=production
```
`npx webpack`：是用来运行本地安装`Webpack`包的。

`./src/main.js`：指定`webpack`从`main.js`文件开始打包，不仅会打包`main.js`,还会将其依赖也一起打包进来。

`--mode=xxx`：指定打包环境

#### 5. 观察输出文件
默认`webpack`会将文件打包输出到`dist`目录下。

### 小结
`webpack`本身功能比较少，只能处理`js`资源，一旦遇到`css`等其它资源就会报错。

所以学习`webpack`，主要学习如何处理其他资源。

## 基本配置
在开始使用`webpack`之前，需要对`webpack`的配置进行了解。
### 核心概念
**1. entry(入口)**

指示`webpack`从哪个入口文件开始打包。

**2. output(输出)**

指示`webpack`打包完的文件输出到哪里去，如何命名等。

**3. loader(加载器)**

`webpack`本身只能处理`js`、`json`等资源，其它资源需要借助`loader`，`webpack`才能解析。

**4. plugins(插件)**

扩展`webpack`的功能。
**5. mode(模式)**

主要有两种模式：
+ 开发模式：`development`
+ 生产模式：`production`

### webpack配置文件
根目录下新建文件`webpack.config.js`
```js
const path = require("path");
module.exports = {
    //入口
    entry:'./src/main.js',
    //输出
    output:{
        //输出路径
        path:path.resolve(__dirname,'dist'),
        //输出文件名
        filename:'main.js'
    },
    //loader加载器
    module:{
        rules:[]
    },
    //plugins 插件
    plugins:[],
    // mode 模式
    mode:'development'
}
```
`webpack`是基于`Node.js`运行的，所以采用 Common.js 模块化规范。

运行`npx webpack`。


## 开发模式介绍
此模式下主要做两件事：
+ 编译代码，使浏览器能识别运行；
    + 开发时有样式资源、字体图标、图片资源、html资源等，`webpack`默认都不能处理这些资源，所以要加载配置来编译这些资源；

+ 代码质量检查，树立代码规范
    + 提前检查代码的一些隐患，让代码运行时能更加健壮；
    
    + 提前检查代码规范和格式，统一团队编码风格，让代码更优雅美观。


## 处理样式资源
使用`webpack`处理`CSS`、`Less`、`Sass`、`Scss`等样式资源。








