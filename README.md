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

