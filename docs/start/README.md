---
title: 快速上手
---

## 核心配置

### 1.入口

入口起点(entry point)指示 webpack 应该使用哪个模块，来作为构建其内部依赖图的开始。进入入口起点后，webpack 会找出有哪些模块和库是入口起点（直接和间接）依赖的。

```bash
module.exports = {
  entry: './src/index.js'
};
```

### 2.出口

output 属性告诉 webpack 在哪里输出它所创建的 bundles，以及如何命名这些文件

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


在更高层面，在 webpack 的配置中 loader 有两个目标：
* test 属性，用于标识出应该被对应的 loader 进行转换的某个或某些文件。
* use 属性，表示进行转换时，应该使用哪个 loader。
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

### 4.插件(plugins)

插件是 webpack 最重要的功能之一，目的在于解决 loader 无法实现的其他事。

#### 由于插件可以携带参数/选项，你必须在 webpack 配置中，向 plugins 属性传入 new 实例。

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