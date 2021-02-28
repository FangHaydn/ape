# Vue-Router

## 模式

### 1、History模式

利用浏览器history的Api实现，监听pushState实现， 链接无hash后缀比较美观，但是需要服务端配置调整资源映射

### 2、Hash模式

通过监听hashChange事件修改渲染不同组件

### 3、



## this.$router 和 this.$route区别

### this.$router

this.$router是VueRouter实例，包含VueRouter的实例方法，push， back， replace

### this.$route

this.$route是本页面路由信息实例，包括路由信息patch，query， name等
