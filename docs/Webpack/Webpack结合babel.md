# Webpack结合babel

1. 首先根据`webpack基础配置`新建项目。
2. 在项目里面添加 `babel.config.json` 文件。
3. 下载`babel`所需要的包 `npm install --save-dev babel-loader @babel/core @babel/preset-env`
4. 修改配置文件 `webpack.config.js` 和`babel.config.json`

    + `webpack.config.js`增加如下配置

        ```js
        module: {
            rules: [{ 
                test: /\.js$/, 
                exclude: /node_modules/, 
                loader: "babel-loader" 
            }]
        }
        ```

    + `babel.config.json`中添加如下配置

        ```json
        {
            "presets": ["@babel/preset-env"]
        }   
        ```

5. 可以如`babel的简单配置`中添加测试文件。（其中`package.js`文件不用按照`babel的简单配置`中修改）
