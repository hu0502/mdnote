1、config/index.js（解决出现index.html空白页面问题：build下的）

```javascript
assetsPublicPath: './',
```

2、utils.js（解决图片资源问题）

```javascript
if (options.extract) {
  return ExtractTextPlugin.extract({
  use: loaders,
  fallback: 'vue-style-loader',
  publicPath:'../../'
  })
} else {
	return ['vue-style-loader'].concat(loaders)
}
```

3、不能使用

mode：'history' 默认hash