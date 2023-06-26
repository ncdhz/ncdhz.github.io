# Vue 整合 Electron

1. 全局安装`vue-cli` `npm install @vue/cli -g`
2. 通过 `vue-cli`创建 `vue`工程
    + 执行 `vue create test` 会出现两个选项

        ```
        ❯ default (babel, eslint) 
          Manually select features 
        ```

    + 选择默认的（你也可以根据自己的需要选择第二个然后自己设置想要的依赖）
    + 进入 `test` 目录`cd test`
3. 执行 `vue add electron-builder` 安装 `electron`
    + 这里会出现选择 `electron` 版本（可以根据需求选择）

        ```
        Choose Electron Version 
          ^7.0.0 
          ^8.0.0 
        ❯ ^9.0.0 
        ```

4. 执行 `npm run electron:serve` 就可以看到效果了
5. 其中可能会存在 `@types/node` 版本问题可以执行`npm install @types/node@^12.0.12 --dev`解决
