---
title: 快速上手
---

## 核心配置

### 1.入口(entry)

入口起点指示 webpack 应该使用哪个模块，来作为构建其内部依赖图的开始。进入入口起点后，webpack 会找出有哪些模块和库是入口起点（直接和间接）依赖的。

```bash
module.exports = {
  entry: './src/index.js'
};
```

### 2.出口(output)

告诉 webpack 在哪里输出它所创建的 bundles，以及如何命名这些文件

```bash
module.exports = {
    output:{
        path:path.resolve(__dirname,'dist'),
        filename:'static/js/[name].bundle.js',
        publicPath:'/'
    },
};
```

### 3.loader

loader 让 webpack 能够去处理那些非 JavaScript 文件（webpack 自身只理解 JavaScript）。loader 可以将所有类型的文件转换为 webpack 能够处理的有效模块，然后你就可以利用 webpack 的打包能力，对它们进行处理。

* test 属性，用于标识出应该被对应的 loader 进行转换的某个或某些文件。(必须)
* use 属性，表示进行转换时，应该使用哪个 loader。(必须)
* exclude, 表示哪些目录中不要进行loader
* include, 表示哪些目录中需要进行loader

```bash
module.exports = {
    module: {
        rules: [{ 
                test: /\.scss$/, 
                use: 'sass-loader' 
            },{
                test: /\.css$/,
                use: [
                    { loader: 'style-loader' },
                    {
                        loader: 'css-loader',
                        options: {
                            modules: true
                        }
                    }
                ]
            }
        ]
    }
};
```
::: warning 注意
在 webpack 配置中定义 loader 时，要定义在 module.rules 中，而不是 rules。然而，在定义错误时 webpack 会给出严重的警告。
:::

### 4.插件(plugins)

插件是 webpack 最重要的功能之一，目的在于解决 loader 无法实现的其他事。
loader 被用于转换某些类型的模块，而插件则可以用于执行范围更广的任务。插件的范围包括，从打包优化和压缩，一直到重新定义环境中的变量。插件接口功能极其强大，可以用来处理各种各样的任务。

* 想要使用一个插件，你只需要 require() 它，然后把它添加到 plugins 数组中
* 你也可以在一个配置文件中因为不同目的而多次使用同一个插件
* 由于插件可以携带参数/选项，你必须在 webpack 配置中，向 plugins 属性传入 new 实例。

```bash

const HtmlWebPackPlugin = require("html-webpack-plugin");

module.exports = {
    plugins:[
        new HtmlWebPackPlugin({
            template: "./src/index.html",
            filename: "./index.html"
        })
    ]
};
```

#### Loaders和Plugins区别

Loaders和Plugins是两个完全不同的东西，可以这么来说，loaders是在打包构建过程中用来处理源文件的（JSX，Scss，Less..），一次处理一个，插件并不直接操作单个文件，它直接对整个构建过程其作用。

* loader的定义为在模块加载时的预处理文件，运行在打包文件之前。
* plugins的定义为处理loader无法处理的事物，在整个编译周期都起作用。