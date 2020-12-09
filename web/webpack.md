# Webpack

[toc]

[中文文档](https://webpack.docschina.org/concepts/)

> 本质上，webpack 是一个用于现代 JavaScript 应用程序的静态模块打包工具。当webpack处理应用程序时，它会在内部构建一个 依赖图(dependency graph)，此依赖图对应映射到项目所需的每个模块，并生成一个或多个 bundle.


## 核心概念

主要是一下内容 入口(entry)、输出(output)、loader、插件(plugin)、模式(mode)、浏览器兼容性(browser compatibility)、环境(environment)

### entry

入口起点(entry point)指示 webpack 应该使用哪个模块，来作为构建其内部 依赖图(dependency graph) 的开始。进入入口起点后，webpack 会找出有哪些模块和库是入口起点（直接和间接）依赖的。


### output

output 属性告诉 webpack 在哪里输出它所创建的 bundle，以及如何命名这些文件。

### loader

webpack 只能理解 JavaScript 和 JSON 文件，这是 webpack 开箱可用的自带能力。loader 让 webpack 能够去处理其他类型的文件，并将它们转换为有效 模块，以供应用程序使用，以及被添加到依赖图中。

### plugin

插件可以用于执行范围更广的任务。包括：打包优化，资源管理，注入环境变量。

### mode

通过选择 development, production 或 none 之中的一个，来设置 mode 参数，你可以启用 webpack 内置在相应环境下的优化。其默认值为 production。

## 参考

[带你深度解锁Webpack系列(基础篇)](https://juejin.cn/post/6844904079219490830)

[带你深度解锁Webpack系列(优化篇)](https://juejin.cn/post/6844904093463347208)

[带你深度解锁Webpack系列(进阶篇)](https://juejin.cn/post/6844904084927938567)