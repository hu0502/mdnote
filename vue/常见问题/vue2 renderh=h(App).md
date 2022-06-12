####render函数是渲染一个视图，然后提供给el挂载，如果没有render那页面什么都不会出来

```javascript
new Vue({
  router,
  store,
  //components: { App }  vue1.0的写法
  render: h => h(App)    vue2.0的写法
}).$mount('#app')
```



#### vue.2.0的渲染过程：

+ 首先需要了解这是 es6的语法，表示 Vue 实例选项对象的 render 方法作为一个函数，接受传入的参数 h 函数，返回 h(App) 的函数调用结果。

+ 其次，Vue 在创建 Vue 实例时，通过调用 render 方法来渲染实例的 DOM 树。

+ 最后，Vue 在调用 render 方法时，会传入一个 createElement 函数作为参数，也就是这里的 h 的实参是 createElement 函数，然后 createElement 会以 APP 为参数进行调用。

  结合一下官方文档的代码便可以很清晰的了解Vue2.0 render:h => h(App)的渲染过程。   

```javascript
render: function (createElement) {
    return createElement(
        'h' + this.level,   // tag name 标签名称
        this.$slots.default // 子组件中的阵列
    )
}
```

