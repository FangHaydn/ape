通用性较好方案

* 使用伪类+scale

```css
.border-1px {
    position: relative;
}
.border-1px::before {
    content: "";
    position: absolute;
    left: 0;
    top: 0;
    width: 200%;
    border: 1px solid #000;
    color: red;
    height: 200%;
    -webkit-transform-origin: left top;
    transform-origin: left top;
    -webkit-transform: scale(0.5);
    transform: scale(0.5);
    pointer-events: none; /* 防止点击触发 */
    box-sizing: border-box;
    @media screen and (min-device-pixel-ratio: 3),
        (-webkit-min-device-pixel-ratio: 3) {
        width: 300%;
        height: 300%;
        -webkit-transform: scale(0.33);
        transform: scale(0.33);
    }
}

/* 右边单边框 */
.border-1px-right {
    position: relative;
}
.border-1px-2::before {
    content: "";
    position: absolute;
    width: 1px;
    height: 100%;
    right: 0;
    border-right: 1px solid #000;
    -webkit-transform: scaleX(0.33);
    transform: scaleX(0.5);
    pointer-events: none;
    box-sizing: border-box;
    @media screen and (min-device-pixel-ratio: 3),
        (-webkit-min-device-pixel-ratio: 3) {

        -webkit-transform: scaleX(0.33);
        transform: scaleX(0.33);
    }
}
```


* svg + border-image 兼容较好，但是换颜色麻烦