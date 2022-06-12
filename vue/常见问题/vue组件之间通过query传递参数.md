### 需求

  从 **任务列表** 进入 **任务详情** ，向详情页传递当前 **mission_id** 值   

#### 路由关系

```javascript
	//查看任务列表    {
        path: '/worklist',
        name: 'worklist',
        component: worklist,
    },
    //任务详情    {
        path: '/workdetails',
        name: 'workdetails',
        meta: {
            requireAuth: true //需要先登录        
        },
        component: workdetails
    }
```

#### worklist.vue传递

```html
<router-link :to="{
      path: '/workdetails',
      query: {
          mission_id: index.mission_id //当前missionID
       }
    }"
class="aaalistlink">
　　<el-button icon="el-icon-search" circle type="danger" class="check"></el-button>
</router-link>
```

####workdetails.vue 中接收参数   

```vue
var data = {
    mission_id :  this.$route.query.mission_id, //传递的missionID
    user_id :  this.$store.state.user_id
}
```

