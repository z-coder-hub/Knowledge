### Webpack是什么？

> 它是一种前端资源构建工具，一个静态模块打包器。可以将`js\css\json\img\less`作为模块处理，通过静态分析构建，最后形成静态资源（bundle）
>
> 特点：
>
> - webpack本身只能处理`js/json`文件，不能识别`css\img`等文件
> - 生产环境、开发环境都可以识别`ES6`语法
> - 生产环境比开发环境多出一个代码压缩的过程
>



### 打包的流程

> 通过入口文件`index.js`将引入的依赖引入形成`chunk`块，通过`chunk`编译不同的文件，这个过程称为打包。
>
> 最后输出的文件称为`bundle`



### 五大核心

#### `Entry`

> 入口文件，告诉webpack要从那个文件开始打包

#### `Output`

> 定义输出到哪里，如何命名

#### `Loader`

> 将一些语法转换成浏览器可识别的语法

#### `Plugins`

> 插件，包含一些优化、压缩等

#### `Mode`

> 模式，分为生产模式和开发模式
>
> development
>
> 开发模式
>
> 
>
> production
>
> 生产模式



### Loader

#### 关于样式的`loader`

> 解析模块化的`css`，将样式加载到`head`标签的`style`标签中

##### `style-loader`



> 解析`css`文件，将`css`文件通过`css-loader`以模块化的形式加载到静态文件中

##### `css-loader`

##### `less-loader`

##### `sass-loader`

##### `scss-loader`

##### 在`webpack.config.js`中如何配置

```js
// webpack.config.js
module.exports = {
    module:{
        rulse:[
            {
                test:/\.css$/,//通过正则匹配.css的文件
                
                use:[//要使用的loader插件，以css为例
                    // 从右向左执行
                    'style-loader',
                    'css-loader'
                ]
            }
        ]
    }
}
```

##### 注意点

**`less\sass\scss`的`loader`要先执行，之后执行`css-loader`，最后执行`style-loader`**

1. `Less/Sass/Scss Loader`：首先，Webpack会将Less/Sass/Scss文件传递给相应的预处理器loader（如less-loader、sass-loader或node-sass），将其转换为普通的CSS文件。
2. `css-loader`：接下来，Webpack将转换后的CSS文件传递给css-loader。css-loader主要负责解析CSS文件之间的依赖关系，处理@import和url()等语句，并将最终的CSS代码输出。
3. `style-loader`：最后，Webpack将使用style-loader将生成的CSS代码注入到HTML文件中，实现样式的动态加载。style-loader会将CSS代码以style标签的形式插入到HTML的head标签中。

### Plugins

#### `html-webpack-plugin`

```js
const HtmlWebpackPlugin = require('html-webpack-plugin')
// 先引入需要的plugin
// 因为plugin是一个类，所以要通过new实例才可以使用
plugins: [
    new HtmlWebpackPlugin({
        template: './src/index.html' // 指定要打包的html文件。如果没有template，打包之后会是一个空白的html文件
    })
]
```



### `webpack.config.js`

> 作用：指示`webpack`怎么干活、配置文件
>
> 所有的构建工具都基于`Nodejs`平台，模块化使用的`commonjs`规范

**基本配置**

```js
const { resolve } = require('path')
module.exports = {
    // 入口文件
    entry: './src/index.js',

    // 出口文件
    output: {
        // 打包之后的文件名
        filename: 'build.js',
        // 绝对路径
        path: resolve(__dirname, 'build')
    },

    // loader配置
    module: {
        rules: []
    },

    // plugins配置
    plugins: [

    ],

    // 模式配置
    mode: 'development' // 开发模式
}
```



### 优化

#### HotModuleReplacement

默认情况在文件修改后webpack会重新打包所有的代码，打开热模块加载就只会打包修改部分的代码

HotModuleReplacement（HMR/热模块替换）：在程序运行中，替换、添加或删除模块，而无需重新加载整个页面。

1. 基本配置

```javascript
module.exports = {
  // 其他省略
  devServer: {
    host: "localhost", // 启动服务器域名
    port: "3000", // 启动服务器端口号
    open: true, // 是否自动打开浏览器
    hot: true, // 开启HMR功能（只能用于开发环境，生产环境不需要了）
  },
};
```

#### OneOf

使用原因，webpack匹配loader是依次匹配，虽然匹配成功也会继续向下匹配

使用结果：只能匹配上一个 loader, 剩下的就不匹配了。

##### 怎么用

```javascript
module: {
    rules: [
      {
        oneOf: [
          把所有的loader放到oneOf中
        ]
      }
   ]
}
```



#### Include/Exclude

使用原因：可以只处理部分文件或者不处理部分文件
