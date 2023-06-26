# electron 由于网络原因下载缓慢

1. 在 `yarn` 中
    + 新建 `.yarnrc` 文件 `touch .yarnrc`
    + 添加以下配置到 `.yarnrc` 文件

        ```shell
        electron_mirror "https://npm.taobao.org/mirrors/electron/"
        ```

2. 在 `npm` 中
    + 新建 `.npmrc` 文件 `touch .npmrc`
    + 添加以下配置到 `.npmrc` 文件

        ```shell
        electron_mirror = "https://npm.taobao.org/mirrors/electron/"
        ```

3. 在 `mac` 系统中 `.yarnrc` 和 `.npmrc` 存放的位置

    ```
    ~/.yarnrc
    ~/.npmrc
    ```
