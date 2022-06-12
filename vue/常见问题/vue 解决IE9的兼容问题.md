#### vue 解决ie9的兼容问题

当vue遇见ie9的时候，部署到服务器之后，打开居然是一片空白，vue是支持ie9的，这个时候就需要来一波兼容了，参考尤大的解答 https://github.com/vuejs-templates/webpack/issues/260

首先：

```javascript
npm install --save babel-polyfill
```

然后在**main.js**中的最前面引入babel-polyfill

```javascript
import 'babel-polyfill'
```

在index.html 加入以下代码（非必须）

```html
<meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1">
```

在**config**中的**webpack.base.conf.js**中,修改编译配置

```javascript
entry:{
    app:['babel-polyfill','./src/main.js']      
}
```

当然，如果你只用到了 axios 对 promise进行兼容，可以只用 es6-promise  

```javascript
npm install es6-promise --save
```

在 **main.js** 中的最前面引入   

```javascript
import 'es6-promise/auto'
```

完成以上配置，ie9兼容就完成了   