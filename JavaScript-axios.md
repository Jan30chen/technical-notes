# JavaScript-axios

> 参考：http://axios-js.com/zh-cn/docs/index.htm

[Axios](https://github.com/axios/axios) 是一个基于 promise 的 HTTP 库，可以用在浏览器和 node.js 中，

## 安装

使用 npm:

```
$ npm install axios
```

**注意：**安装之后还需要使用`import`或者`require`语句导入

使用 bower:

```
$ bower install axios
```

使用 cdn:

```html
<script src="https://unpkg.com/axios/dist/axios.min.js"></script>
```

## 特点

- 从浏览器中创建 [XMLHttpRequests](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest)
- 从 node.js 创建 [http](http://nodejs.org/api/http.html) 请求
- 支持 [Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise) API
- 拦截请求和响应
- 转换请求数据和响应数据
- 取消请求
- 自动转换 JSON 数据
- 客户端支持防御 [XSRF](http://en.wikipedia.org/wiki/Cross-site_request_forgery)

## 基础用法

### 自定义请求-`axios(config)`

通过配置参数对象使用 axios

```js
// 发送 POST 请求
axios({
  method: 'post',
  url: '/user/12345',
  data: {
    firstName: 'Fred',
    lastName: 'Flintstone'
  }
});
```

config 的更多字段，参见[详细配置字段](##详细配置字段)

### 默认请求-`axios(url[, config])`

使用默认参数，只指示请求地址，默认请求方法为GET

```js
// 发送 GET 请求（默认的方法）
axios('/user/12345');
```

## 别名请求方法

每一种请求方法都有相关别名方法可以直接使用，相关请求方法参见[HTTP 请求方法](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Methods)

+ axios.request(config)：
+ **axios.get(url[, config])**
+ axios.delete(url[, config])
+ axios.head(url[, config])
+ **axios.post(url[, data[, config]])**
  + 注意第二个参数将作为`data`属性传入
+ axios.put(url[, data[, config]])
+ axios.patch(url[, data[, config]])

## 并发请求

通过以下两个方法搭配，完成并发请求

+ axios.all(iterable)：将多个请求合作一处，类似`Promise.all()`，只有当请求都完成后才执行`then`方法，参数为数组形式
+ axios.spread(callback)：`then`方法获取的是响应值的数组，该函数将响应值按请求顺序拆为独立参数返回

```js
function getUserAccount() {
  return axios.get('/user/12345');
}

function getUserPermissions() {
  return axios.get('/user/12345/permissions');
}

axios.all([getUserAccount(), getUserPermissions()])
  .then(axios.spread((acct, perms) => {
    // 两个请求都完成后
  }));
```

**注意：**两个方法都是 axios 的静态方法，其实例无法使用这些方法

## 实例

### 创建实例-`axios.create([config])`

使用自定义配置新建一个 axios 实例

```js
var instance = axios.create({
  baseURL: 'https://some-domain.com/api/',
  timeout: 1000,
  headers: {'X-Custom-Header': 'foobar'}
});
```

### 实例方法

作为 axios 的实例，也都具有别名请求方法。指定的配置将与实例的配置合并

## 请求详细配置

以下字段中：

+ 只有`url`是必填的，其余都可选
+ 如果`method`没有指定，则默认为`GET`

```js
const a = {
  
  url: '/user',   // 用于请求的服务器地址
  method: 'get',  // 创建请求时使用的方法，默认是get
  baseURL: 'https://some-domain.com/api/', // 自动加在url前面，除非url是一个绝对URL

  /* 
    允许在向服务器发送前，修改请求数据，只能用在 Post、Put与Patch 这几个请求方法
    后面数组中的函数必须返回一个字符串，或 ArrayBuffer，或Stream */
  transformRequest: [function (data) { return data }],
  // 在传递给 then/catch 前，允许修改响应数据
  transformResponse: [function (data) { return data }],

  // headers 是即将被发送的自定义请求头
  headers: {'X-Requested-With': 'XMLHttpRequest'},

  // 即将与请求一起发送的 URL 参数，必须是一个无格式对象(plain object)或 URLSearchParams 对象
  params: { ID: 12345 },
  // 负责params序列化的函数
  // (e.g. https://www.npmjs.com/package/qs, http://api.jquery.com/jquery.param/)
  paramsSerializer: function(params) {
    return Qs.stringify(params, {arrayFormat: 'brackets'})
  },

  /*   
    作为请求主体被发送的数据，只适用于Post、Put与Patch请求方法
    在没有设置transformRequest时，必须是以下类型之一：
    - string, plain object, ArrayBuffer, ArrayBufferView, URLSearchParams
    - 浏览器专属：FormData, File, Blob
    - Node 专属： Stream */
  data: { firstName: 'Fred' },

  // 服务器响应的数据类型，取值为：json(默认)、arraybuffer、blob、document、、text、stream
  responseType: 'json',
  
  timeout: 1000,  // 指定请求超时的毫秒数，超时将中断请求，0表示无超时时间

  withCredentials: false, // 表示跨域请求时是否需要使用凭证，默认为 false

  // 允许自定义处理请求，以使测试更轻松
  // 返回一个 promise 并应用一个有效的响应 (查阅 [response docs](#response-api)).
  adapter: function (config) {
    /* ... */
  },

  // 表示应该使用 HTTP 基础验证，并提供凭据
  // 这将设置一个 Authorization 头，覆写掉现有的任意使用 headers 设置的自定义 Authorization头
  auth: {
    username: 'janedoe',
    password: 's00pers3cret'
  },

  // 用作 xsrf token 的值的cookie的名称
  xsrfCookieName: 'XSRF-TOKEN', // default
  // 承载 xsrf token 的值的 HTTP 头的名称
  xsrfHeaderName: 'X-XSRF-TOKEN', // 默认的

  // 允许为上传处理进度事件
  onUploadProgress: function (progressEvent) {
    // 对原生进度事件的处理
  },
  // 允许为下载处理进度事件
  onDownloadProgress: function (progressEvent) {
    // 对原生进度事件的处理
  },

  // 定义允许的响应内容的最大尺寸
  maxContentLength: 2000,

  // validateStatus 定义对于给定的HTTP 响应状态码是 resolve 或 reject promise 。如果 validateStatus 返回 true (或者设置为 null 或 undefined)，promise 将被 resolve; 否则，promise 将被 rejecte
  validateStatus: function (status) {
    return status >= 200 && status < 300; // 默认的
  },

  // maxRedirects 定义在 node.js 中 follow 的最大重定向数目
  // 如果设置为0，将不会 follow 任何重定向
  maxRedirects: 5, // 默认的

  // httpAgent 和 httpsAgent 分别在 node.js 中用于定义在执行 http 和 https 时使用的自定义代理。允许像这样配置选项：
  // keepAlive 默认没有启用
  httpAgent: new http.Agent({ keepAlive: true }),
  httpsAgent: new https.Agent({ keepAlive: true }),

  // 定义代理服务器的主机名称和端口
  // auth 表示 HTTP 基础验证应当用于连接代理，并提供凭据
  // 这将会设置一个 Proxy-Authorization 头，覆写掉已有的通过使用 header 设置的自定义 Proxy-Authorization 头
  proxy: {
    host: '127.0.0.1',
    port: 9000,
    auth: {
      username: 'mikeymike',
      password: 'rapunz3l'
    }
  },

  // cancelToken 指定用于取消请求的 cancel token
  // （查看后面的 Cancellation 这节了解更多）
  cancelToken: new CancelToken(function (cancel) { })
}
```

## 请求响应

### 响应结构

请求返回的响应结构如下：

```js
{
  // `data` 由服务器提供的响应
  data: {},
  // `status` 来自服务器响应的 HTTP 状态码
  status: 200,
  // `statusText` 来自服务器响应的 HTTP 状态信息
  statusText: 'OK',
  // `headers` 服务器响应的头
  headers: {},
  // `config` 是为请求提供的配置信息
  config: {}
}
```

### 获取响应对象

#### 获取`resolved`

当获取一个`resolved`状态，使用`then`方法，直接获得返回对象后根据字段获取

```js
axios.get('/user/12345')
  .then(function(response) {
    console.log(response.data);
    console.log(response.status);
    console.log(response.statusText);
    console.log(response.headers);
    console.log(response.config);
   });
```

#### 获取`rejected`

使用 `catch` 方法或作为`then`方法的第二参数时，获取返回的错误对象的`response`字段获取实际对象，再通过字段获取

```js
axios.get('/user/12345')
  .catch(function (error) {
    if (error.response) {
      // 请求已发出，但服务器响应的状态码不在 2xx 范围内
      console.log(error.response.data);
      console.log(error.response.status);
      console.log(error.response.headers);
    } else {
      // Something happened in setting up the request that triggered an Error
      console.log('Error', error.message);
    }
    console.log(error.config);
  });
```

## 配置默认值

### 全局配置

```js
axios.defaults.baseURL = 'https://api.example.com';
axios.defaults.headers.common['Authorization'] = AUTH_TOKEN;
axios.defaults.headers.post['Content-Type'] = 'application/x-www-form-urlencoded';
```

### 实例配置

```js
// 创建实例时设置配置的默认值
var instance = axios.create({
  baseURL: 'https://api.example.com'
});

// 在实例已创建后修改默认值
instance.defaults.headers.common['Authorization'] = AUTH_TOKEN;
```

### 配置优先级

当某个实例的同个配置被不同赋值，按照优先级进行覆盖重写

以下优先级由低到高：

+ 库的默认值，位于`lib/defaults.js`中
+ 实例的`defaults`属性，可以分为两种情况：
  + 实例创建时，`create`方法中的配置
  + 实例创建后，通过`defaults`属性再次修改配置值
  + 需要通过实例的`defaults`属性修改，修改`axios.defaults`全局配置是不会改变实例的配置的
+ 实例请求时的配置，即`config`参数

## 拦截器

### 创建

在请求或响应被`then`或`catch`处理前拦截它们

```js
// 添加请求拦截器
axios.interceptors.request.use(function (config) {
    // 在发送请求之前做些什么
    return config;
  }, function (error) {
    // 对请求错误做些什么
    return Promise.reject(error);
  });

// 添加响应拦截器
axios.interceptors.response.use(function (response) {
    // 对响应数据做点什么
    return response;
  }, function (error) {
    // 对响应错误做点什么
    return Promise.reject(error);
  });
```

### 删除

```js
var myInterceptor = axios.interceptors.request.use(function () {/*...*/});
axios.interceptors.request.eject(myInterceptor);
```

## 错误处理

如何获取错误时的响应对象，参见 [响应结构-获取`rejected`](####获取`rejected`)

此外，可以使用 `validateStatus` 配置选项定义一个自定义 HTTP 状态码的错误范围。

```js
axios.get('/user/12345', {
  validateStatus: function (status) {
    return status < 500; // 状态码在大于或等于500时才会 reject
  }
})
```

## 取消请求？

此处待补充