安装

```shell
npm i --save ant-design-vue@next
```

在线笔记：

https://24kcs.github.io/vue3_study/00_%E8%AF%BE%E7%A8%8B%E4%BB%8B%E7%BB%8D.html

+ vue3中的provide与inject

  只能由父组件 ===> 孙子组件，不能向上传递。因为父组件的setup先执行，向上注入接收的是undefined 



+ vue3 使用provide注入执行方法？

  注入时可以使用ref和reactive添加数据响应式

+ suspense

+ 计算属性可以是包含get和set方法的对象，也可以是回调函数

  计算属性不写 set 就只能 get 方法不能修改



面试常用

vue3支持vue2的大部分特性：向下兼容

vue中设计了组合API代替了Vue2中的options API，复用性更好

更好的支持TS

**最主要：**

Vue3中使用了Proxy配合Reflect代替了Vue2中Object.defineProperty()方法实现数据的响应式(数据代理)

重写了虚拟DOM，速度更快

新增了新的组件：Fragment(片段) / Teleport(瞬移) / Suspense(loading)

设计了新的脚手架工具，vite

options API 和 Composition API 的对比：

Options API 的问题：

+ 在传统的Vue Options API中，新增或者修改一个需求，就需要分别在data、methods、computed中修改，业务量过大时整页代码上下移动

Composition API ：

让模块化代码更整洁，函数式编程



