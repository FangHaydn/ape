# 防抖
> 过滤频繁（基于定义的delay时间）操作的中间状态，只取最后结果
```javascript
function debounce(timeout, callback) {
    let timer

    return function() {
        if (timer !== null) {
            clearTimeout(timer)
            timer = null
        }
        let that = this
        timer = setTimeout(function(){
            callback.apply(that, arguments)
        }, timeout)
    }
}
```


# 节流
> 基于时间频率的抽样
```javascript
function throttle(delay, callback) {
    let timestrap = 0
    let that = this
    return function() {
        if ((Date.now() - timestrap) > delay) {
            timestrap = Date.now()
            callback.apply(that, arguments)
        }
    }
}
```