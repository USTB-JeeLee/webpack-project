## webpack 可以进行 0 配置

- 📦 工具 -> 输出后的结果(JS 模块)
- 📦(支持 js 的模块化)

## 手动配置 webpack

- webpack 的 0 配置有点弱
- 默认配置文件的名字 webpack.config.js

## 转化 ES6 语法

````js
      {
        test: /\.js$/, // normal
        use: {
          loader: "babel-loader",
          options: {
            // 用babel-loader 需要把es6 -> es5
            presets: ["@babel/preset-env"],
            plugins: [
              ["@babel/plugin-proposal-decorators", { legacy: true }],
              ["@babel/plugin-proposal-class-properties", { loose: false }],
              "@babel/plugin-transform-runtime"
            ]
          }
        },
        include: path.resolve(__dirname, "src"),
        exclude: /node_modules/
      },
```

## 处理js语法及校验
```js
      {
        test: /\.js$/,
        use: {
          loader: "eslint-loader",
          options: {
            enforce: "pre" // previous post
          }
        }
      },
````

## 全局变量引入问题

三种方式：

- expose-loader 暴露到全局上
- providePlugin 给每个人提供一个\$
- 引入不打包的方式

## 打包图片

- 在 js 中创建图片来引入
- 在 css 中引入 background: url("")
- <img src="" alt="">
    file-loader 完整的图片
    url-loader base64📦，减少HTTP请求
    html-withimg-loader

```js
      {
        test: /\.html$/,
        use: "html-withimg-loader"
      },
      {
        test: /\.(png|jpg|gif)$/,
        // 做一个限制，当我们的图片小于多少K时，用base 64转化
        // 否则用file-loader产生真实的图片
        use: {
          loader: "url-loader",
          options: {
            limit: 200 * 1024
          }
        }
      },
```

## 📦多页应用
```js
module.exports = {
  // 多入口
  entry: {
    home: "./src/index.js",
    other: "./src/other.js"
  },
  output: {
    // name -> home a
    filename: "[name].js",
    path: path.resolve(__dirname, "dist")
  },
  plugins: [
    new HtmlWebpackPlugin({
      template: "./src/index.html",
      filename: "index.html",
      chunks: ["home"]
    }),
    new HtmlWebpackPlugin({
      template: "./src/index.html",
      filename: "other.html",
      chunks: ["other", "home"]
    })
  ]
};
```

