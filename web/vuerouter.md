# Vue-Router

## 模式

### 1、History模式

利用浏览器history的Api实现，监听pushState实现， 链接无hash后缀比较美观，但是需要服务端配置调整资源映射

### 2、Hash模式

通过监听hasChange事件修改渲染不同组件

### 3、