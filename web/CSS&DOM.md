绝⼤大多数情况下我们总是让JavaScript在DOM载⼊入后再开始执⾏行行。不不
管是直接⽤用 DOM API 实现还是使⽤用 jQuery，最终都
是DOMContentLoaded事件在起作⽤用。本⽂文讨论⼀一个我们习以为常却很
少了了解的问题：样式⽂文件的载⼊入会延迟脚本执⾏行行，以及
**DOMContentLoaded** 事件的触发。

# DOMContentLoaded 事件

## ⻚页⾯面⽂文档（DOM）完全加载并解析完毕之后，会触发

DOMContentLoaded事件，HTML⽂文档不不会等待样式⽂文件，图⽚片⽂文件，
Iframe⻚页⾯面的加载。但DOM树已被创建，多数JavaScript已经操作
DOM并完成功能了了。
This (DOMContentLoaded) event fires after the HTML code has been
fully retrieved from the server, the complete DOM tree has been
created and scripts have access to all elements via the DOM API. –
molily.de

然⽽而在绝⼤大多数场景下，样式⽂文件的载⼊入会延迟DOMContentLoaded事
件的触发。其实这样的⾏行行为正是开发者所希望的，为什什么呢？
事实上，⽼老老版本的IE中DOMContentLoaded事件存在兼容性问题。参⻅见：
兼容所有浏览器器的 DOM 载⼊入事件。

# 浏览器器为何延迟 DOMContentLoaded

## 对于很多脚本⽽而⾔言，它们被编写时就希望在样式载⼊入之后再开始执

⾏行行。JavaScript的作者往往会假设CSS规则已经⽣生效，尤其是在进⾏行行⼀一
些显示相关的操作时，⽐比如需要得到DOM元素的位置和⼤大⼩小的场景。
事实上，在多数浏览器器中DOMContentLoaded事件的触发会考虑到外部
样式⽂文件（CSS）的载⼊入，以及在HTML中脚本标签和样式标签的相对
位置。如果脚本位于样式之后，浏览器器通常会认为该脚本依赖于样式


## 的渲染结果，也就更更倾向于延迟脚本的执⾏行行（直到样式渲染结束）。

# 不不同浏览器器的⾏行行为

既然浏览器器有时会延迟DOMContentLoaded事件，但是何时会延迟
DOMContentLoaded事件，还取决于⾏行行内脚本还是外部脚本，以及脚本
与样式标签的相对位置。不不同的浏览器器渲染引擎也有不不同的⾏行行为。
在http://molily.de/domcontentloaded/⼀一⽂文中对该问题有详尽的阐述和
测试，本⽂文取其结论。
下表描述了了各种情况下脚本是否会被延迟执⾏行行，进⽽而延迟触发
DOMContentLoaded事件。

# HTML5 标准及最佳实践

其实DOMContentLoaded是Firefox中最先提出的，此后JavaScript社区
发现它确实⽐比load事件（要求所有资源完全载⼊入）更更好，于是Apple和
Opera相继开始⽀支持该事件。但不不同浏览器器的实现⽅方式有所区别，于是
产⽣生了了上表所示的复杂情况。
在HTML5标准中情况有所好转：DOMContentLoaded是⼀一个纯DOM事
件，与样式表⽆无关。与此同时，HTML5要求：
脚本执⾏行行前，出现在当前之前的必须完全载⼊入。
脚本执⾏行行会阻塞DOM解析。
这样的话，假如脚本和样式⼀一起放在HTML中，DOM解析到标签时会

```
渲染引擎 样式表之前的脚本 样式表之后的外部脚本
Presto (Opera) 否 否
Webkit (Safari, Chrome) 否 是
Gecko (Firefox) 否 是
Tri d e n t (MSIE) 是
```

## 阻塞DOM解析，开始如下操作：

## 1. 获取当前的脚本⽂文件；

## 2. 获取并载⼊入前⾯面的所有。

## 3. 执⾏行行当前脚本⽂文件。

## 这些操作完成之后才能继续进⾏行行DOM解析，解析完毕时再触发

DOMContentLoaded事件。如果将样式和脚本都放到中，会使浏览器器在
渲染前载⼊入并执⾏行行所有样式和脚本。⻚页⾯面的显示延迟会很⾼高，多数情
况下⽤用户体验都很糟糕。因此在HTML5标准的HTML⻚页⾯面中，最佳实
践应当是：所有样式放在中；所有脚本放在最后。
jQuery⽂文档中也推荐这样的实践⽅方式。

# 参考阅读
[MDN load 事件：](https://developer.mozilla.org/en-US/docs/Web/Events/load)
[MDN DOMContentLoaded事件：](https://developer.mozilla.org/zh-CN/docs/Web/Events/DOMContentLoaded)



