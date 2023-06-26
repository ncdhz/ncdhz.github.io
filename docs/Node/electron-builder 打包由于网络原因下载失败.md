# electron-builder 打包由于网络原因下载失败

1. `vue` 结合 `electron` 项目解决方案
    + 新建 `vue.config.js`
    + 在里面添加如下配置

    ```js
    module.exports = {
        pluginOptions: {
            electronBuilder: {
                builderOptions: {
                    "electronDownload": {
                        // 下载路径改为taobao
                        "mirror": "https://npm.taobao.org/mirrors/electron/"
                    }
                }
            }
        }
    }
    ```

2. `electron` 项目
    + 找到 `package.json` 添加如下配置

    ```js
    build: {
        "electronDownload": {
              "mirror": "https://npm.taobao.org/mirrors/electron/"
        }
    }
    ```

3. 可以把相应二进制包放入对应的缓存目录
    + 进入 `https://npm.taobao.org/mirrors/electron/` 下载对应的包

    ```
    Linux: $XDG_CACHE_HOME or ~/.cache/electron/
    macOS: ~/Library/Caches/electron/
    Windows: $LOCALAPPDATA/electron/Cache or ~/AppData/Local/electron/Cache/
    ```
