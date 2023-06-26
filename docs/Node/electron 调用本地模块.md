# electron 调用本地模块

1. 由于安全问题，electron调用本地模块会出现错误

    ```
    // 我的机器出现的错误
    // 可能不同机器出现的错误不同
    // Cannot find module
    ```

2. 解决办法

    ```js
    // 原本使用的
    require('/path/to/module')
    // 改
    import { remote } from 'electron'
    remote.require('/path/to/module')
    ```
