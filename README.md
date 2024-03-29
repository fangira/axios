# axios

- ### 配置参数
```js
axios.create({
  // 基础路径
  baseURL:"http://localhost:8080",
  // 超时时长
  timeout:1000,
  // 请求路径
  url:"/city",
  // 方法get,post,put,patch,delete
  method:"get",
  // 请求头
  headers:{token:"abc"},
  // 把参数放在url?后面。放在query
  params:{a:"query"},
  // 把参数放在请求体
  data:{b:"body"}
})


```

- ### 安装
```cmd
npm i axios
```

- ### 引入

```vue
# a.vue
<script>
import axios from 'axios';

export default {
}
<script>
```

- ### 基本使用

```js
// get请求

axios({
  method:"get",
  url:"/city",
  params:{id:1},  
}).then(res=>{console.log(res)})


// post 请求
axios({
  method:"post",
  url:"/city",
  data:{id:1},  
}).then(res=>{console.log(res)})
```


- ### 并发请求
同时进行多个请求，并统一处理返回值
```
axios.all([
  // 请求一
  axios.get("/a");
  // 请求二
  axios.get("/b");
  // 有多少个请求，此处的形参就对应有多少个
]).then(axios.spread(aRes,bRes)=>{
  // do something
  console.log(aRes,bRes);
})
```

- ### axios实例
比如后端接口有多个，并且超时时长不一
```js
// 创建一个axios实例
let axios1 = axios.create({
  baseURL："http://localhost:8080",
  // 设置超时时间
  timeout:1000
})
let axios2 = axios.create({
  baseURL："http://localhost:1234",
  timeout:200
})


// 调用（类似直接调用axios）
axios1.get("/city").then(res=>{
  console.log(res);
})
```

- ### 三种配置
```js
// 全局配置
axios.default.baseURL = "http://localhost:8080";
axios.default.timeout = 1000;

// 实例配置 (会覆盖)
let aaa = axios.create(); // 如果实例化过程create中不传参，使用的还是全局配置
aaa.default.timeout = 500; // 此时该实例aaa的超时为500ms
// 请求配置(会覆盖)
aaa.get("/city",{
  timeout:300 // 此时会覆盖之前的实力配置与全局配置，最后的超时为300，baseURL为"http://localhost:8080"
})
```

- ### 拦截器
在请求前或者响应前进行拦截
```js
  // 请求前添加token到请求头
  let instance = axios.create(); // 创建一个实例,不要污染全局
  instance.interceptors.request.use(config=>{
    config.headers.token='xxx';
    return config;
  })
  
  // 请求过程添加loading动画
  let instance = axios.create(); // 创建一个实例,不要污染全局
  instance.interceptors.request.use(config=>{
    $("#modal").show();
    return config;
  })
  instance.interceptors.respond.use(res=>{
    $("#modal").hide();
    return res;
  })
  
```

- ### 处理错误
```js
  let instance = axios.create();
  instance.interceptors.request.use(config=>{
    return config;
  },err=>{
    // 请求错误一般是 401超时，404 Not Find
    // 错误提示
    $("#tips").show();
    setTimeout(()=>{
      $("#tips").hide();
    },2000);
    return Promise.reject(err);
  })
  instance.interceptors.respond.use(res=>{
    return res;
  },err=>{
    // 响应错误一般是 500系统错误，502系统重启
    // 错误提示
    $("#tips").show();
    setTimeout(()=>{
      $("#tips").hide();
    },2000);
    return Promise.reject(err);
  })
```

- ### 取消请求
```js
let source = axios.CancelToken.source(); // 实例化source对象
axios.get("/city",{
  cancelToken:source.token  // 把source.token放进请求设置的参数中
}).then(res=>{
  console.log(res);
}).catch(err=>{
  console.log(err);
})

// 取消请求(errMsg)
source.cancel("这里的字符串会进入err里面");

```
