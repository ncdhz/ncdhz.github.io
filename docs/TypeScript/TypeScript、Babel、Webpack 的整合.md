# TypeScript、Babel、Webpack 的整合

## 整合 TypeScript 和 Babel

1. 下载所需要的包

    ```ts
    // npm
    npm install --save-dev typescript @babel/core @babel/cli @babel/plugin-proposal-class-properties @babel/preset-env @babel/preset-typescript
    // yarn
     yarn add -D typescript @babel/core @babel/cli @babel/plugin-proposal-class-properties @babel/preset-env @babel/preset-typescript
    ```

2. 创建 `tsconfig.json`

    ```shell
    ./node_modules/typescript/bin/tsc --init --declaration --allowSyntheticDefaultImports --target esnext --outDir lib
    
    # TypeScript还提供了一个--declarationDir选项，该选项指定生成的声明文件（.d.ts文件）的输出目录。
    ```

3. 创建 `.babelrc`

    ```json
    {
        "presets": [
          "@babel/preset-env",
          "@babel/preset-typescript"
        ],
        "plugins": [
          "@babel/plugin-proposal-class-properties"
        ]
    }
    ```

4. 添加以下配置到 `package.json` 的 `scripts`

    ```json
    // npm
    "scripts": {
        "type-check": "tsc --noEmit",
        "type-check:watch": "npm run type-check -- --watch",
        "build": "npm run build:types && npm run build:js",
        "build:types": "tsc --emitDeclarationOnly",
        // 源目录 src 输出目录 lib
        "build:js": "babel src --out-dir lib --extensions \".ts,.tsx\" --source-maps inline"
    }
    // yarn
    "scripts": {
        "type-check": "tsc --noEmit",
        "type-check:watch": "yarn type-check -- --watch",
        "build": "yarn build:types && npm run build:js",
        "build:types": "tsc --emitDeclarationOnly",
        "build:js": "babel src --out-dir lib --extensions \".ts,.tsx\" --source-maps inline"
    }
    ```

5. 如果不想整合 `Webpack` 已经可以告一段落了

## 继续整合 Webpack

1. 加入依赖

    ```shell
    // npm
    npm install --save-dev webpack webpack-cli babel-loader
    // yarn
    yarn add -D webpack webpack-cli babel-loader
    ```

2. 创建 `webpack.config.js`

    ```js
    // 添加以下内容到 webpack.config.js
    
    var path = require('path');

    module.exports = {
        // Change to your "entry-point".
        entry: './src/index',
        output: {
            path: path.resolve(__dirname, 'dist'),
            filename: 'app.bundle.js'
        },
        resolve: {
            extensions: ['.ts', '.tsx', '.js', '.json']
        },
        module: {
            rules: [{
                // Include ts, tsx, js, and jsx files.
                test: /\.(ts|js)x?$/,
                exclude: /node_modules/,
                loader: 'babel-loader',
            }]
        },
        // 用于剔除代码压缩包
        optimization:{
          minimize: false
        } 
    };
    ```

3. 添加以下配置到 `package.json` 的 `scripts`

    ```json
    "bundle": "webpack"
    ```

4. 现在你可以通过以下命令使用它了

    ```shell
    npm run bundle
    ```
