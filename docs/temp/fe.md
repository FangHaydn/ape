

## 浏览器

### 浏览器缓存

* Cookie：由于http是无状态的，cookie自动放在http请求头中，用于缓存标记用户身份信息，一般限制4K大小

* sessionStorage：回话页面级本地存储，5MB限制，回话关闭就失效

* localStorage：同源页面都可以访问到的，本地持久缓存，5MB限制

* SQL/DB：webSQL已废弃，indexDB一种key-value存储的类似NoSQL数据库，IE10

### 跨域

#### 跨域及解决方案

跨域是因为浏览器的同源安全策略导致的

只有协议、域名和端口都一致的请求才被允许

__解决跨域问题主要由3种：__

* Jsonp  script标签请求无同源策略限制，兼容性好，只支持get请求

* 服务器代理

    一般线上使用服务器代理实现请求转发，一般是和web同源服务器做请求代理，另一种是非同源代理需要做CORS处理

* CORS，跨域资源共享

    兼容IE10， IE8、9需要是使用XDomainRequest，只支持get、post

```js
简单请求
// request-header
{
	Origin: http://baidu.com
}
// response-header 
{
	// 或者是使用通配符 *
 	Access-Control-Allow-Origin: http://baidu.com 
}

非简单请求，
1、请求方法是以下三种方法之一：
HEAD
GET
POST
2、HTTP的头信息不超出以下几种字段：
Accept
Accept-Language
Content-Language
Last-Event-ID
Content-Type：只限于三个值application/x-www-form-urlencoded、multipart/form-data、text/plain
不满足以上任一条件，出于安全发起预检请求OPTIONS
// request-header 
{
	Origin: http://baidu.com 
	Access-Control-Request-Method: PUT,
	Access-Control-Request-Headers: XXXX, DDDD  // 非简单请求的自定义header字段名列表
}
// response-header 
{
	// 或者是使用通配符 *
 	Access-Control-Allow-Origin: http://baidu.com
    Access-Control-Allow-Methods: PUT, POST, GET
	Access-Control-Allow-Headers: XXXX, DDDD
	Access-Control-Max-Age：86400 //有效期，24小时内不需要再发起预检
}

withCredentials
// request-header
{
	Origin: http://baidu.com
	withCredentials: true
}
// response-header 
{
 	Access-Control-Allow-Origin: http://baidu.com  // 不能使用通配符
 	Access-Control-Allow-Credentials: true  //允许发送Cookie和http认证信息
}
```



#### Hybrid开发

App 混合开发实现（Android）

URL拦截 & 执行JS是在Android和iOS上通用性和兼容性较好的方式； 然后在JS层做一个JSBridge的封装和回调管理

[🔗 JSBridge实战](https://juejin.im/post/6844903702721986568)

__方案一：__

JS中动态加载链接（协议），Native中做协议拦截处理

```java
class MyWebViewClient extents WebViewClient {
  public boolean shouldOverrideUrlLoading(WebView webview, String url) {
      if (url.startWith("jockey")) {
        // processUri() 处理约定协议相对应的Native处理逻辑
        return true; // 拦截处理该协议
      }
      return false;
  }
}
WebView webview.setWebViewClient(new MyWebViewClient());
```

__方案二：__

使用JavaScriptInterface

```java
class JsBridgeApi {
	@JavascriptInterface
  public void sayHello(String params) {
     // Native handle
  }
}
WebView webview.addJavascriptInterface(new JsBridgeApi(), "JsBridgeMethods"); // 将native代码方法注入JS中
```

```javascript
window.JsBridgeMethods.sayHello('hello')
```

__回调方案：__

```java
WebView webview.loadUrl("javascript:Jockey.trigger(id,'world')");
```

```javascript
var Jockey = {
    callbacks :{1: {callback}},
    id: 1,
	trigger: function(id, data) {
        this.callbacks[id].callback(data);
    },
    invoke: function(url, callback) {
        // send JsBridge messge
        this.callbacks[this.id] = {callback};
        this.id++;
    }
}
```

__方案三：__

```java
// 该方案需要Android 4.4及以上
@TargetApi(Build.VERSION_CODES.KITKAT)
public void Android2JsHaveParmHaveResult2(View view) { 
 	mWebView.evaluateJavascript("sumToJava(3,4)", new ValueCallback<String>() {
    @Override 
    public void onReceiveValue(String Str) {   
    	   // str返回值
    });
}
```



## JavaScript

#### 修改this指向

bind(), call(), apply()

#### eventloop



### 原型链

#### 怎么理解原型和原型链

JS是基于原型的编程，属于OOP一种，JS通过原型实现实例对象创建，通过原型链实现继承

#### 原型链继承

```javascript
// 寄生组合
function A() { }
function B() {
	A.call(this)
}
B.prototype = Object.create(A.prototype)
B.prototype.constructor = B
```



### 闭包

#### 怎么理解闭包

解释1：函数对象通过作用域链相互关联起来，函数内部变量都可以保持在函数的作用域中，有权访问另一个函数作用域中的变量

解释2：一个绑定执行环境的函数



### 函数式编程



### ES6

#### Promise

[Promise自定义](./Promise.md)



### TypeScript



## Html

### BFC

### 外边距合并

### 动画

### Canvas



## CSS

### 预处理器

**CSS缺点**

- 语法不够强大，比如无法嵌套书写，导致模块化开发中需要书写很多重复的选择器；
- 没有变量和合理的样式复用机制，使得逻辑上相关的属性值必须以字面量的形式重复输出，导致难以维护

**预处理器提供 CSS 缺失的样式层复用机制、减少冗余代码，提高样式代码的可维护性，提高开发效率**



#### Less

#### Sass







## VueJS

### Vue

#### Vue 2.x响应式实现

#### Vue 3.x响应式实现

#### $nextTick

#### 组件通讯



### Vue-Router

#### Hash模式原理

#### History模式原理

#### 路由懒加载

#### 非官方路由



### Vuex



## 工程化

### Webpack

#### webpack打包流程

##### Vue Cli4



## 优化

### 移动端适配

* 等比缩放 rem

* 弹性布局 flex

### Vue优化

#### 首屏加载优化

#### 运行时优化

#### SEO优化





## Http

[http](./http.md)

