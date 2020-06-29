### 步骤

1. 创建项目目录并进行npm初始化

   ```
   npm init
   ```

2. 初始化为git仓库

   ```
   git init
   ```

3. 编写一个静态页面pulic/index.html用于展示

4. 集成babel，用于将es6以上代码转化为es5代码

   1. 添加相关依赖

      ```
      yarn add @babel/core @babel/cli @babel/preset-env @babel/preset-react --dev
      ```

   2. 添加babel配置文件.babelrc

      ```
      {
        "presets": ["@babel/env", "@babel/preset-react"]
      }
      ```

5. 集成webpack

   1. 添加webpack相关依赖

      ```
      yarn add webpack webpack-cli webpack-dev-server babel-loader --dev
      ```

   2. 添加webpack配置文件webpack.config.js

      ```
      const path = require("path");
      
      module.exports = {
        entry: "./src/index.js",
        mode: "development",
        module: {
          rules: [
            {
              test: /\.(js|jsx)$/,
              exclude: /(node_modules|bower_components)/,
              loader: "babel-loader",
              options: { presets: ["@babel/env"] }
            },
          ]
        },
        resolve: { extensions: ["*", ".js", ".jsx"] },
        output: {
          path: path.resolve(__dirname, "dist/"),
          publicPath: "/dist/",
          filename: "bundle.js"
        },
        devServer: {
          contentBase: path.join(__dirname, "public/"),
          port: 3000,
          publicPath: "http://localhost:3000/dist/",
          hotOnly: true
        },
      };
      ```

   3. 添加文件src/index.js, 并在public/index.html中引入../dist/bundle.js文件

      ```
      document.body.append("Hello JS")
      ```

   4. 运行yarn webpack，可以看到生成了dist/bundle.js文件，然后使用浏览器打开public/index.html就可以看到效果

6. 使用dev-server

   1. 添加依赖

      ```
      yarn add webpack-dev-server --dev
      ```

   2. 在webpack.config.js中添加dev-server配置

      ```
      module.exports = {
        devServer: {
          contentBase: path.join(__dirname, "public/"),
          port: 3000,
          publicPath: "http://localhost:3000/dist/",
          hotOnly: true,
          open: true
        },
      }
      ```

   3. 添加下面的npm script到package.json, 并执行yarn start

      ```
      "start": "webpack-dev-server --mode development"
      ```

      