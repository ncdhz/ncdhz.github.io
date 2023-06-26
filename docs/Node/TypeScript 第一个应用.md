# TypeScript 第一个应用

1. 安装`Nodejs`并执行`npm install -g typescript`(全局安装typescript，其中包含 `tsc` 命令)
2. 新建文件 `test.ts` `touch test.ts`
3. 在文件中添加如下代码

    ```js
    function test(name: string): string {
        return "Hello " + name;
    }
    const hello:string = test("ncdhz");
    console.log(hello);
    ```

4. 执行 `tsc test.ts` 把`ts`代码转换为`js`代码, 可以通过 `tsc --watch test.ts`来启动`监视模式`也就是不用一直把`ts`代码编译成`js`代码，可以自动完成编译
5. 你会发现当前目录多了一个文件 `test.js`

    ```js
    function test(name) {
        return "Hello " + name;
    }
    var hello = test("ncdhz");
    console.log(hello);
    ```

6. 接着你就可以用`js`方式执行 `test.js`文件了`node test.js`
7. 不过上述方式很繁琐我们可以下载`ts-node`包（`npm i -g ts-node`，其中`ts-node`可以在内部吧`ts`代码转换为`js`代码然后运行并提供了`ts-node`命令）来完成上述工作
8. 直接执行 `ts-node test.ts` 你会得到和上述一样的结果
