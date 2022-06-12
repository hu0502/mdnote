#### 出现问题

> vue解决刷新页面vuex数据、params参数消失的问题。

可以通过几种情况进行传值：

+ 通过路由传值，params或query
+ 通过vuex进行状态管理 $store.state
+ 使用localStorage进行传值

前两种方法会导致页面刷新数据消失。

#### 解决方法

##### 一、路由

+ params

+ query

  params传值刷新页面消失，query不会，两者的区别就在于query会把传递的参数显示在url地址中，在定义路由的时候，给path设定参数。

  ```javascript
  export default [{
      path: '/platform',
      component: Layout2,
      children: [{
          path: '/adminCun/:jum',//这里值用:加参数的写法，jum即为参数，注意一定要用/隔开
          name:'platformRecycleAdminCun',
          meta:{
             title:'管辖村级详情'
             },
          component: resolve =>		 require(['@/view/platform/recycle/admincun'],resolve)
      }]
  }]
  ```

  ```html
  <template slot-scope="props">
    <el-button type="text"  @click="$router.push({name:'platformRecycleAdminCun',params:                 {jum:props.row.jgNum}})">
      {{ props.row.jgName }}
      </el-button>
  </template>
  ```

  

##### 二、==vuex==/localStorage

页面刷新store.state中的数据消失是不可避免的，将store的数据存储在localStorage里。

在App.vue中，created初始化生命周期中写入以下方法：

```javascript
//在页面刷新时触发beforeunload()，将store/vuex中的信息存入localStorage
 window.addEventListener("beforeunload",()=>{
   localStorage.setItem("messageStore",JSON.stringify(this.$store.state))
 })

//在页面加载时读取localStorage里的状态信息
//replaceState()替换store的根状态，再通过对象赋值assign将localStorage进行赋值
 localStorage.getItem("messageStore") &&  this.$store.replaceState(Object.assign(this.$store.state,JSON.parse(localStorage.getItem("messageStore"))));
```