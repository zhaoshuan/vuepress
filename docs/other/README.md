---
title: 其他
---

## 1.模式(mode)

提供 mode 配置选项，告知 webpack 使用相应模式的内置优化。
除了在配置文件里设置，还可以通过命令行参数传入
* 参数：development / production

```bash
# 在 package.json 配置下，运行命令
"scripts": {
    "stag": "webpack --mode development",
    "build": "webpack --mode production"
},

module.exports = {
  mode: 'production'
};
```
## 2.虚拟服务器(devServer)
webpack-dev-server是一个使用了express的Http服务器，它的作用主要是为了监听资源文件的改变，该http服务器和client使用了websocket通信协议，只要资源文件发生改变，就会实时的进行编译。

```bash
# 安装
npm i -D webpack-dev-server 

# 在 package.json 配置下，运行命令会自动在node_modules文件夹找 webapck-dev-server模块。
"scripts": {
    "dev": "webpack-dev-server --mode development --inline --progress",
}

module.exports = {
    devServer: {
        port: 8086,
        host:'localhost',   
        open:false, 
    },
};
```
* hot (热更新)
* host (主机名，默认localhost)
* prot (端口)
* contentBase (路径,默认情况下，它将使用您当前的工作目录来提供内容)
* open (自动打开浏览器)
* proxy (代理)


* [更多参数了解](https://webpack.js.org/configuration/dev-server/)


## 3.选项(resolve)

### resolve.alias
* 模块被其他模块名和路径替代
```bash
module.exports = {
    resolve: {
        alias: {
            '@': path.resolve(__dirname, 'src'),
        }
    },
};
```

* resolve.root 用来配置搜索路径集合。root配置必须是绝对路径，不能存在./app/modules之类的相对路径。
* resolve.modulesDirectory 是指需要向上搜索的目录名称，一般只会是node_modules之类的。
* [更多参数了解](https://webpack.js.org/configuration/resolve)


## 4.webpack4与3的区别

* 零配置，开箱即用
* 新增mode
* modules.loader => modules.rules
* CommonChunksPlugin => SplitChunksPlugin。CommonChunksPlugin已经从webpack4中移除。提取公用代码可以利用SplitChunksPlugin。
* ExtractTextWebpackPlugin => MiniCssExtractPlugin。webpack4使用MiniCssExtractPlugin取代ExtractTextWebpackPlugin。