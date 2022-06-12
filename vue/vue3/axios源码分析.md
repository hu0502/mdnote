### axios使用

#### 安装json-server

##### 安装

```shell
npm install -g json-server
```

##### 新建json

```json
//db.json
{
    "post":[
        {
            "id":1,
            "title":"json-server",
            "author":"typicode"
        }
    ],
    "comments":[
        {
            "id":1,
            "body":"some comment",
            "postId":1
        }
    ],
    "profile" : {
        "name" : "typicode"
    }
}
```

##### json

cd到json-server目录下进行启动操作

```shell
json-server --watch db.json
```

#### axios的基本使用

```javascript
 axios({
     method:'GET',//POST PUT DELETE
     url:'http://localhost:3000/posts',
     data:{}
 }).then(response => {
     console.log(response)
 })
```



#### axios的其它方式

```javascript
btns[0].onclick = function(){
    axios.request({
        methods:'GET',
        url:'http://localhost:3000/comments'
    }).then(reponse => {
        console.log(reponse);
    })
}
btns[1].onclick = function(){
    axios.post(url,params)
        .then(res => {
        console.log(res)
    })
        .catch(err => {
        console.error(err); 
    })
}
```



#### axios请求响应结果的结构

![](H:\code\mdnote\asset\vue\axios\axios_01.png)

+ config：配置对象，请求url、请求类型、请求体等
+ data：响应体的结果，data是对象的原因是axios自动将服务器返回结果进行了json解析，转成了对象方便处理
+ headers：响应头信息
+ request：原生ajax请求对象，保存的是当前axios发送请求时创建的ajax请求对象，即XMLHttpRequest实例对象

#### 配置对象

https://github.com/axios/axios#request-config

#### 默认配置

```javascript
axios.defaults.methods = 'GET';
axios.defaults.baseURL = 'http://localhost:3000' 
```

#### 创建实例对象

```javascript
const dz = axios.create({
    baseURL: 'https://api.apiopen.top',
    timeout: 2000
})
console.log(dz);//封装成对象dz
//1
dz({
    url:'/getJoke'
}).then(res=>{
    console.log(res);
})
//2
dz.get('/getJoke').then(res => {
    console.log(res.data)
})
```

#### 拦截器

请求拦截器：后进先执行

响应拦截器：先进先执行



### axios源码分析

#### 目录结构

![](H:\code\mdnote\asset\vue\axios\axios_02.png)

#### 创建过程

创建一个函数，在函数上添加方法和属性得到最终结果

```javascript
//构造函数
function Axios(config){
    this.defaults = config;//创建default默认属性
    this.intercepters = {
        request:{},
        response:{}
    }
}
//原型上添加相关方法
Axios.prototype.request = function(config){
    console.log('发送AJAX请求类型为'+config.method);
}
Axios.prototype.get = function(config){
    return this.request({method:'GET'});
}
Axios.prototype.post = function(config){
    return this.request({method:'POST'});
}

//声明函数
function createInstance(config){
    //1.实例化一个对象 context.get(),context是对象，不能作为函数使用
    let context = new Axios(config)
    //2.创建请求函数,此时instance是一个函数，且可以instance({})，不能用作对象使用
    let instance = Axios.prototype.request.bind(context)
    //3.将Axios.prototype对象中的方法添加到instance函数对象中
    Object.keys(Axios.prototype).forEach(key=>{
        instance[key] = Axios.prototype[key].bind(context)
    })
    //4.此时instance可作为函数和对象使用
    //5.为instance函数对象添加属性default 与 interceptors
    Object.keys(context).forEach(key=>{
        instance[key] = context[key];
    })
    return instance
}
let axios = createInstance({method:'GET'})
axios({method:'POST'})
axios.get({})
axios.post({})
```

#### 请求发送过程

```request```调用 ```dispatchRequest```=> XHR => 最终返回结果

```javascript
//1.声明一个构造函数
function Axios(config) {
    this.config = config
}
Axios.prototype.request = function (config) {
    //发送请求
    //创建一个promise对象
    let promise = Promise.resolve(config)
    //声明一个数组
    let chains = [dispatchRequest, undefined] //占位
    //调用then方法指定回调
    let result = promise.then(chains[0], chains[1])
    //返回 promise 结果
    return result;
}

//2.dispatchRequest函数 chains[0]
function dispatchRequest(config) {
    //调用适配器发送请求
    return xhrAdapter(config).then(response => {
        //对响应的结果进行转换处理
        return response
    }, error => {
        throw error //throw 抛出异常时，.then()返回结果就是失败的Promise对象
    })
}

//3. adapter适配器
function xhrAdapter(config) {
    console.log('xhrAdapter 函数执行')
    return new Promise((resolve, reject) => {
        //发送Ajax请求
        let xhr = new XMLHttpRequest()
        //初始化
        xhr.open(config.method, config.url)
        //发送
        xhr.send()
        //绑定事件
        xhr.onreadystatechange = function () {
            if (xhr.readyState === 4) {
                //判断成功的条件
                if (xhr.status >= 200 && xhr.status <= 300) {
                    //成功
                    resolve({
                        //配置对象
                        config: config,
                        //响应体
                        data: xhr.response,
                        //响应头
                        headers: xhr.getAllResponseHeaders(),//字符串
                        //xhr的请求对象
                        request: xhr,
                        //状态码
                        status: xhr.status,
                        statusText: xhr.statusText
                    })
                } else {
                    //失败
                    reject(new Error('请求失败，失败的状态码为：' + xhr.status))
                }
            }
        }
    })
}
//4.创建axios函数
let axios = Axios.prototype.request.bind(null)
axios({
    method: 'GET',
    url: 'http://localhost:3000/posts'
}).then(response => {
    console.log(response);
});
```



#### 拦截器实现

```javascript
//构造函数
function Axios(config){
    this.config = config;
    this.interceptors = {
        request: new InterceptorManager(),
        response: new InterceptorManager(),
    }
}
//发送请求  难点与重点
Axios.prototype.request = function(config){
    //创建一个 promise 对象
    let promise = Promise.resolve(config);
    //创建一个数组
    const chains = [dispatchRequest, undefined];
    //处理拦截器
    //请求拦截器 将请求拦截器的回调 压入到 chains 的前面  request.handles = []
    this.interceptors.request.handlers.forEach(item => {
        chains.unshift(item.fulfilled, item.rejected);
    });
    //响应拦截器
    this.interceptors.response.handlers.forEach(item => {
        chains.push(item.fulfilled, item.rejected);
    });

    // console.log(chains);
    //遍历
    while(chains.length > 0){
        promise = promise.then(chains.shift(), chains.shift());
    }

    return promise;
}

//发送请求
function dispatchRequest(config){
    //返回一个promise 队形
    return new Promise((resolve, reject) => {
        resolve({
            status: 200,
            statusText: 'OK'
        });
    });
}

//创建实例
let context = new Axios({});
//创建axios函数
let axios = Axios.prototype.request.bind(context);
//将 context 属性 config interceptors 添加至 axios 函数对象身上
Object.keys(context).forEach(key => {
    axios[key] = context[key];
});

//拦截器管理器构造函数
function InterceptorManager(){
    this.handlers = [];
}
InterceptorManager.prototype.use = function(fulfilled, rejected){
    this.handlers.push({
        fulfilled,
        rejected
    })
}
// 功能测试
// 设置请求拦截器  config 配置对象
axios.interceptors.request.use(function one(config) {
    console.log('请求拦截器 成功 - 1号');
    return config;
}, function one(error) {
    console.log('请求拦截器 失败 - 1号');
    return Promise.reject(error);
});

axios.interceptors.request.use(function two(config) {
    console.log('请求拦截器 成功 - 2号');
    return config;
}, function two(error) {
    console.log('请求拦截器 失败 - 2号');
    return Promise.reject(error);
});

// 设置响应拦截器
axios.interceptors.response.use(function (response) {
    console.log('响应拦截器 成功 1号');
    return response;
}, function (error) {
    console.log('响应拦截器 失败 1号')
    return Promise.reject(error);
});

axios.interceptors.response.use(function (response) {
    console.log('响应拦截器 成功 2号')
    return response;
}, function (error) {
    console.log('响应拦截器 失败 2号')
    return Promise.reject(error);
});

//发送请求
axios({
    method: 'GET',
    url: 'http://localhost:3000/posts'
}).then(response => {
    console.log(response);
});
```

#### 取消请求

在CalcelToken身上维护了一个promise属性，通过Promise成功回调resolve改变promise属性的状态，把此状态暴露到全局，来取消请求。

cancel调用 => resolvePromise调用 => promise属性状态为resolve => then回调 => xhr.abort()完成取消



### axios总结

#### axios与Axios的关系

+ 从语法上来说axios不是Axios的实例
+ 从功能上来说axios是Axios的实例（axios拥有Axios实例对象上的所有方法，通过扩展的方式都加在了原型上）
+ axios是Axios.prototype.request函数bind()返回的函数
+ axios作为对象有Axios原型对象上的所有方法，有Axios对象上所有属性

#### instance和axios的区别

+ 相同
  + 都是一个能发任意请求的==函数==request(config)
  + 也可以作==对象==使用，可以调用各种方法get() | post() | put() | delete()
  + 都有默认配置和拦截器的属性:defaults | interceptors
+ 不同
  + 默认配置可以有所区别
  + instance没有axios后面添加的一些方法如：create() | CancelToken() | all()

#### axios运行的整体流程

![](H:\code\mdnote\asset\vue\axios\axios_03.png)

##### 整体流程

request(config) ==> dispatchRequest(config) ==> xhrAdapter(config)

###### request(config)

将请求拦截器 / dispatchRequest() / 响应拦截器通过promise链串联起来，返回promise

###### dispatchRequest(config)

转换请求数据 ==> 调用xhrAdapter()发送请求 ==> 请求返回后转换响应数据，返回promise

###### xhrAdapter(config)

创建XHR对象，根据config进行相应设置，发送特定请求，并接收响应数据，返回promise



#### axios请求响应拦截器

##### 请求拦截器

+ 在真正发送请求前执行的回调函数
+ 可以对请求进行检查或配置进行特定处理
+ 成功的回调函数，传递的默认是config
+ 失败的回调函数，传递的默认是Error

##### 响应拦截器

+ 在请求得到响应后执行的回调函数
+ 可以对响应数据进行特定处理
+ 成功的回调函数，传递默认是response
+ 失败的回调函数，传递默认是Error

#### 请求/响应数据转换器

##### 请求转换器

对请求头和请求体数据进行特定处理的函数

```javascript
if(utils.isObject(data)){
    setContentTypeIFUnset(headers,'application/json;charset=utf-8');
    return JSON.stringify(data);
}
```

##### 响应转换器

将响应体json字符串解析为js对象或数组的函数

#### 取消未完成的请求

##### 配置cancelToken对象时，保存cancel函数

+ 创建一个用于将来中断请求的cancelPromise
+ 定义了一个用于取消请求的cancel函数
+ 将cancel函数传递出来

##### 调用cancel()取消请求

+ 执行cancel函数，传入错误信息message
+ 内部会让cancelPromise变为成功，且成功的值为一个Cancel对象
+ 在cancelPromise的成功回调中中断请求，并让发请求的promise失败，失败的reason为Cancel对象