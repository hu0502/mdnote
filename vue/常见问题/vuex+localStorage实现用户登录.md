##### store/index.js   

```javascript
// store index.js
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)
// 初始化时用sessionStore.getItem('token'),这样子刷新页面就无需重新登录
const state = {
    username: window.localStorage.getItem('username'),
    token: window.localStorage.getItem('token')
}

const mutations = {
    //将token保存到sessionStorage里，token表示登陆状态
    SET_TOKEN: (state, data) => {
    state.token = data
    window.localStorage.setItem('token', data)
	},
    //获取用户名
    GET_USER: (state, data) => {
        // 把用户名存起来
        state.username = data
        window.localStorage.setItem('username', data)
    },
    //登出
    LOGOUT: (state) => {
        // 登出的时候要清除token
        state.token = null
        state.username = null
        window.localStorage.removeItem('token')
        window.localStorage.removeItem('username')
    }
}

const actions = {
}
export default new Vuex.Store({
    state,
    mutations,
    actions
})
```

##### router/index.js中拦截登陆   或main.js

```javascript
// 注册全局钩子用来拦截导航
router.beforeEach((to, from, next) => {
    const token = store.state.token
    if (to.meta.requireAuth) { // 判断该路由是否需要登录权限
        if (token) { // 通过vuex state获取当前的token是否存在
            //console.log(token)
            next()
        } else {
        //console.log('该页面需要登陆')
            next({
                path: '/login'
                //query:{redirect: to.fullPath}
                //将跳转的路由path作为参数，登录成功后跳转到该路由
            })
        }
    }
    else {
    	next()
    }
})
```

```javascript
//用户个人中心vue组件配置路由
//personalcenter.vue
{
    path:'/personalcenter',
    name:'personalcenter',
    meta:{
        requireAuth:true//进入个人中心路由页面时需要先登录
    },
    component:personalcenter
}
```



##### login.vue   

```javascript
submitForm(formName) {
    var _this = this;
    _this.$refs[formName].validate(valid =>{
	//如果表单验证成功
        if(valid){
            axios.post('/mock/users',{
            username :_this.LoginForm.username,//用户名
            password :_this.LoginForm.password,//密码
        })
    	.then(response =>{
            if (response.status === 200){
                var apiData = response.data.data;
                //在数据返回成功后用this.$store.commit来更新vuex里的数据
                _this.$store.commit('SET_TOKEN', response.headers.authorization)
                _this.$store.commit('GET_USER', apiData.username)
                _this.$message({
                    message: '登陆成功',
                    type: 'success'
                })
                _this.$router.push({
                name:'personalcenter'
                // query:{username:apiData.username}
                })
            }else{
            	console.log("请求失败")
            	}
            })
        }else {
            alert("请正确填写用户名和密码！")
            return false;
        }
	})
}
```

##### 个人中心.vue

进入此页面之前先判断用户是否登录，登陆成功调取localStorage的信息，防止页面刷新后信息消失   

```vue
<!--personalcenter.vue-->
<div class="personalcenter">
    <p style="z-index:10000;">{{this.$store.state.username}}</p>
    <el-button type="primary" @click="logout()">注销登录</el-button>
</div>

<!--header.vue-->
<li v-if="!this.$store.state.username">
	<router-link to="/login"><span>登录</span></router-link>
</li>
<li v-else>
	<router-link to="/personalcenter"><span>个人中心</span></router-link>
</li>
```

注销登录：

```javascript
logout: function () {
    this.$store.commit('LOGOUT');
    this.$message({
        message: '注销成功',
        type: 'success'
    })
    this.$router.push({ name:'home'})
}
```

