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
        entry: "./src/index.tsx",
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

   3. 添加文件src/index.tsx, 并在public/index.html中引入../dist/bundle.js文件

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

7. 集成react

   1. 添加react相关依赖

      ```
      yarn add react react-dom
      ```

   2. 替换src/index.tsx

      ```
      import React from "react";
      import ReactDOM from "react-dom";
      import App from "./App.tsx";
      ReactDOM.render(<App />, document.getElementById("root"));
      ```

   3. 添加src/App.tsx

      ```
      import React, { Component} from "react";
      
      class App extends Component{
        render(){
          return(
            <div>
            <h1> Hello React! </h1>
          </div>
        );
        }
      }
      
      export default App;
      ```

   4. 顺便把src/index.html中无用的Hello World标题删掉

8. 解析并使用css，直到现在，我们的项目仍然无法使用css样式。

   1. 添加css-loader让webpack解析css代码，添加style-loader把css-loader解析后的代码挂载到html中

      ```
      yarn add style-loader css-loader --dev
      ```

   2. 配置style-loader和css-loader, 将以下配置添加到webpack.config.js中

      ```
      module.exports = {
       module: {
       	rules: [
            {
              test: /\.css$/,
              use: ["style-loader", "css-loader"]
            }
       	]
       }
      }
      ```

   3. 创建src/App.css并在App.js中引入App.css

      ```
      .title{
          color: red;
      }
      ```

   4. 修改src/App.js如下：

      ```
      import React, { Component} from "react";
      import "./App.css"
      
      class App extends Component{
        render(){
          return(
            <div>
            <h1 className="title"> Hello React! </h1>
          </div>
        );
        }
      }
      
      export default App;
      ```

9. 集成Typescript

   1. 添加相关依赖, 其中awesome-typescript-loader是为了让webpack转译ts代码文件，因为webpack默认只会处理js代码

      ```
      yarn add typescript awesome-typescript-loader --dev
      ```

   2. 添加Typescript配置文件tsconfig.json

      ```
      {
          "compilerOptions": {
              "outDir": "./dist/",
              "sourceMap": true,
              "noImplicitAny": true,
              "module": "commonjs",
              "target": "es5",
              "jsx": "react"
          },
          "include": [
              "./src/**/*"
          ]
      }
      ```
      
   3. 修改webpack.config.js如下
   
      ```
      const path = require("path");
      
      module.exports = {
      	//change===
        entry: "./src/index.tsx",
        mode: "development",
        module: {
          rules: [
            {
              test: /\.(js|jsx)$/,
              exclude: /(node_modules|bower_components)/,
              loader: "babel-loader",
              options: { presets: ["@babel/env"] }
            },
            {
              test: /\.css$/,
              use: ["style-loader", "css-loader"]
            },
            //+++++
            {
              test: /\.tsx?$/,
              loader: "awesome-typescript-loader"
            },
          ]
        },
        //change===
        resolve: { extensions: [".js", ".ts", ".tsx", ".json", ".css"] },
        output: {
          path: path.resolve(__dirname, "dist/"),
          publicPath: "/dist/",
          filename: "bundle.js"
        },
        devServer: {
          contentBase: path.join(__dirname, "public/"),
          port: 3000,
          publicPath: "http://localhost:3000/dist/",
          hotOnly: true,
          open: true
        },
      };
      
      ```
   
   4. 将src中的js文件改为tsx或ts文件
   
   5. 安装react相关包的声明文件依赖
   
      ```
      yarn add @types/react @types/react-dom --dev
      ```
   
      



