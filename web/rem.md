## 适配

### flex

基于flex+px，可使用于纯文本的H5适配方案



### rem

em是相对字体单位，根据父节点字体比例计算

rem是root-em，根据root节点html的字体比例计算

vw是viewport（视窗）的单位， 100vw = windowWidth;

vw在移动端兼容(IOS 8+, Android 4.4+）
rem在移动端兼容(IOS 4.1+, Android 2.1+)
以下第二种的兼容性会高一些


```html
<!DOCTYPE html>
<html lang="zh">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        /* rem + vw 混用方案 */
        /* 给根元素设置默认基于视窗宽度vw比例值的字体大小，后面可以直接使用rem单位*/
        :root {
            /* 100vw = screenWidth; (32/375)*100 = 8.533vw */
            font-size: 13.333333vw;
        }
        /* 适配大屏，设置默认的字体大小 */
        @media screen and (min-width: 750px) {
            :root {
                font-size: 100px;
            }
        }
        button {
            font-size: 0.32rem;
        }
    </style>
</head>
<body>
</body>
</html>
```



```javascript
function resize() {
    // 纯rem方案
    // 屏幕适配方案一：根据根节点html的fonts-size等比缩放， 按比例换算的地方用rem单位
    // rem (root-em)
    var offsetWidth = document.documentElement.offsetWidth;
    var fontSize = 32;
    if (offsetWidth > 750) {
        offsetWidth = 750;
    }
    document.documentElement.style.fontSize = (fontSize * (offsetWidth / 375)).toFixed(2) + 'px';
}

window.onload = resize;
window.onresize = resize;

```



