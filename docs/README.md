---
home: true
actionText: 开始
actionLink: /start/
features:
- title: 模块化
  details: 一切皆模块化，模块可以是css/js/imsge/font等等，当 webpack 处理应用程序时，它会递归地构建一个依赖关系图，其中包含应用程序需要的每个模块
- title: 依赖管理
  details: 方便引用第三方模块，让模块更容易复用、避免全局注入导致的冲突、避免重复加载或者加载不必要的模块
- title: 打包
  details: 通过一个给定的主文件，从这个文件开始找到你项目的所有依赖文件，使用loaders处理他们，最后打包为一个浏览器可以识别的js文件
footer: 快看前端 | 学习 
---

## 依赖图
依赖 其实就是 当一个模块引入另一个模块的时候，我们说这个模块依赖了另一个模块

![image](http://owiseman.com/imgs/webpack.png)

## 核心概念
从 webpack v4.0.0 开始，可以不用引入一个配置文件。然而，webpack 仍然还是高度可配置的。

1. 入口(entry)
2. 输出(output)
3. loader
4. 插件(plugins)


## 安装

```bash
# 初始化
npm init -y
# 安装依赖
npm i -D webpack webpack-cli
```
::: warning 注意
node支持的版本必须>=6.11.5 建议使用 nodejs 的当前稳定版, [点击进入官网](https://nodejs.org/zh-cn/)

:::

