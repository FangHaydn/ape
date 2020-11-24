# 防抖

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

```javascript
function throttle(delay, callback) {
    let timestrap = 0
    return function() {
        let that = this
        if ((Date.now() - timestrap) > delay) {
            timestrap = Date.now()
            callback.apply(that, arguments)
        }
    }
}
```