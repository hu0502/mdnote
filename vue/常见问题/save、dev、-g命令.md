```javascript
npm install moduleName # 安装模块到项目目录下

npm install -g moduleName # -g 的意思是将模块安装到全局，具体安装到磁盘哪个位置，要看 npm config prefix 的位置。

npm install -save moduleName # -save 的意思是将模块安装到项目目录下，并在package文件的dependencies节点写入依赖。

npm install -save-dev moduleName # -save-dev 的意思是将模块安装到项目目录下，并在package文件的devDependencies节点写入依赖。
```



### npm install 

+ 安装模块到项目node_modules目录下；   
+ 不会将模块依赖写入devDependencies或dependencies 节点；   
+ 运行 `npm install `初始化项目时不会下载模块。   

### npm install -g

+ 安装模块到全局，不会在项目node_modules目录中保存模块包；   
+ 不会将模块依赖写入devDependencies或dependencies 节点；     
+ 运行 `npm install `初始化项目时不会下载模块。   

### npm install --save

+ 安装模块到项目node_modules目录下；   
+ 会将模块依赖写入dependencies 节点；         
+ 运行 `npm install` 初始化项目时，会将模块下载到项目目录下；   
+ 项目部署后也需要用的模块

### npm install --save-dev

+ 安装模块到项目node_modules目录下；   
+ 会将模块依赖写入dependencies 节点；         
+ 运行 `npm install` 初始化项目时，会将模块下载到项目目录下；   
+ 开发模式下需要用到的包或模块，项目部署后不需要

### 总结

devDependencies 节点下的模块是我们在开发时需要用的，比如项目中使用的 gulp ，压缩css、js的模块。这些模块在项目部署后是不需要的，所以使用 --save-dev 的形式安装。像 express 这些模块是项目运行必备的，应该安装在 dependencies 节点下，所以应该使用 --save 的形式安装。