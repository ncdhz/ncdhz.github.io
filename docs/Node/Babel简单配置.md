# Babel简单配置

1. 新建文件夹`demo` `mkdir demo`
2. 初始化 `cd demo` `npm init -y`
3. 下载Bable依赖包 `npm install --save-dev @babel/core @babel/cli @babel/preset-env` 和 `npm install --save @babel/polyfill`
4. 创建 `babel.config.json` `touch babel.config.json` 添加如下内容

    ```json
    {
      "presets": [
        [
          "@babel/env",
          {
            "targets": {
              "edge": "17",
              "firefox": "60",
              "chrome": "67",
              "safari": "11.1",
            },
            "useBuiltIns": "usage",
          }
        ]
      ]
    }
    ```

5. 新建文件夹 `mkdir src`
6. 新建文件 `cd src` `touch index.js` `touch add.js`

    ```js
    // index.js
    
    import {add} from './add'
    console.log(add(2,3));
    ```

    ```js
    // add.js
    
    export const add = (x,y)=>{
        return x+y;
    }
    ```

7. 在`package.json`的`script`中添加`"build": "babel src -d lib"` 返回`demo`目录运行 `npm run build` 在`lib`目录下就可以看见转译好的文件了。
