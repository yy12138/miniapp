当我们访问某个地址页面时，有时会要求我们重新登录后再访问该页面，也就是身份认证失效了，如token丢失了，或者是token依然存在本地，但是却失效了，所以单单判断本地有没有token值不能解决问题。此时请求时服务器返回的是401错误，授权出错，也就是没有权利访问该页面。 
我们可以在发送所有请求之前和操作服务器响应数据之前对这种情况过滤。

```
// JWT 用户权限校验，判断 TOKEN 是否在 localStorage 当中
router.beforeEach(({name}, from, next) => {
  // 获取 JWT Token
  if (localStorage.getItem('JWT_TOKEN')) {
    // 如果用户在login页面
    if (name === 'login') {
      next('/');
    } else {
      next();
    }
  } else {
    if (name === 'login') {
      next();
    } else {
      next({name: 'login'});
    }
  }
});


// http request 拦截器
axios.interceptors.request.use(
  config => {
    if (localStorage.JWT_TOKEN) {  // 判断是否存在token，如果存在的话，则每个http header都加上token
      config.headers.Authorization = `token ${localStorage.JWT_TOKEN}`;
    }
    return config;
  },
  err => {
    return Promise.reject(err);
  });

// http response 拦截器
axios.interceptors.response.use(
  response => {
    return response;
  },
  error => {
    if (error.response) {
      console.log('axios:' + error.response.status);
      switch (error.response.status) {
        case 401:
          // 返回 401 清除token信息并跳转到登录页面
          store.commit('LOG_OUT');
          router.replace({
            path: 'login',
            query: {redirect: router.currentRoute.fullPath}
          });
      }
    }
    return Promise.reject(error.response.data);   // 返回接口返回的错误信息
  });

Vue.prototype.$http = axios;
```


```
watch: {
    // 监控路由
    $route (to, from) {
        // 如果从游戏风格页跳转过来，需要重新获取风格数据
        if (from.fullPath === '/gamestyle') {
            this.getStyleList();
        }
    }
}
```

vue object 在赋值的给另个一个变量的时候，如果变量需要添加属性，则需在最原始赋值的对象添加这个属性，否则将丢失Observer

js获取当前url 的相对路径

```
var url = document.location.toString();
var arrUrl = url.split("//");
var start = arrUrl[1].indexOf("/");
var relUrl = arrUrl[1].substring(start);
if(relUrl.indexOf("?") != -1){
  relUrl = relUrl.split("?")[0];
}
return relUrl;
```

vue 页面加载顺序，建议 template style script 先加载style 不阻塞样式

JS 会阻塞后续 DOM 解析以及其它资源(如 CSS，JS 或图片资源)的加载。
CSS 不会阻塞后续 DOM 结构的解析，不会阻塞其它资源(如图片)的加载，但是会阻塞 JS 文件的加载。
现代浏览器很聪明，会进行 prefetch 优化，浏览器在获得 html 文档之后会对页面上引用的资源进行提前下载。(注意仅仅只是提前下载)


