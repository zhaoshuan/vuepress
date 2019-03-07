---
title: 其他
---

## 1.模式(mode)

提供 mode 配置选项，告知 webpack 使用相应模式的内置优化。
除了在配置文件里设置，还可以通过命令行参数传入
* 参数：development / production
* 生产模式下：启用了 代码压缩、作用域提升（scope hoisting）、 摇晃树(tree-shaking)，使代码最精简
* 开发模式下：相较于更小体积的代码，提供的是打包速度上的优化

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
* [scope hoisting 了解](https://ssshooter.com/2019-02-20-webpack-scope-hoisting/)
* [tree-shaking 了解](https://juejin.im/post/5a4dc842518825698e7279a9)


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

## 3. 自定义打包策略（optimization）
optimization是webpack4新增的，主要是用来让开发者根据需要自定义一些优化构建打包的策略配置，
现在webpack4使用optimization.splitChunks来进行代码的拆分，使用optimization.runtimeChunk来提取webpack的runtime代码，引入了新的cacheGroups概念。并且webpack4中optimization提供如下默认值，官方称这种默认配置是保持web性能的最佳实践，不要手贱去修改，就算你要改也要多测试（官方就是这么自信）。

* minimize：true/false,告诉webpack是否开启代码最小化压缩，
* minimizer：自定js优化配置，会覆盖默认的配置，结合UglifyJsPlugin插件使用，
* removeEmptyChunks: bool 值，它检测并删除空的块。将设置为false将禁用此优化，
* nodeEnv：它并不是node里的环境变量，设置后可以在代码里使用
* splitChunks ：取代了CommonsChunkPlugin，自动分包拆分、代码拆分
* runtimeChunk: 提取 webpack 运行时代码,它可以设置为：boolean、Object
该配置开启时，会覆盖 入口指定的名称！！！

```bash
module.exports = {
  optimization: {
    minimize: env === 'production' ? true : false, //是否进行代码压缩
    splitChunks: {
      chunks: "async",
      minSize: 30000, //模块大于30k会被抽离到公共模块
      minChunks: 1, //模块出现1次就会被抽离到公共模块
      maxAsyncRequests: 5, //异步模块，一次最多只能被加载5个
      maxInitialRequests: 3, //入口模块最多只能加载3个
      name: true,
      cacheGroups: {
        default: {
          minChunks: 2,
          priority: -20
          reuseExistingChunk: true,
        },
        vendors: {
          test: /[\\/]node_modules[\\/]/,
          priority: -10
        }
      }
    },
    runtimeChunk {
      name: "runtime"
    }
  }
}
```

## 4.选项(resolve)

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


## webpack4与3的区别

* 零配置，开箱即用
* 新增mode
* modules.loader => modules.rules
* CommonChunksPlugin => SplitChunksPlugin。CommonChunksPlugin已经从webpack4中移除。提取公用代码可以利用SplitChunksPlugin。
* ExtractTextWebpackPlugin => MiniCssExtractPlugin。webpack4使用MiniCssExtractPlugin取代ExtractTextWebpackPlugin。
* 使用babel-loader 转化ES6->ES5时不再需要 .babelrc 配置文件
* 官方宣传webpack4能够提升构建速度60%-98%

## 总结
* webpack4和前代来比，精简了很多配置，写法上也更佳优雅和便于理解了，不过老实说0配置在大多数情况并不实用，配置这东西多了复杂，少了不灵活，还是需要一个度的
* loader作为webpack的灵魂，插件就是它的核心，相比之下多研究这两个的开发和用法才是重点
* webpack5快发布了，留给大伙学习4的时间不多了