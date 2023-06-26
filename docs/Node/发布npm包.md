# 发布npm包

1. 首先到[npm官方网站](https://www.npmjs.com/)注册账号。
2. 新建一个文件夹`mkdir demo`
3. 进入文件夹 `cd demo`
4. 执行 `npm init` 需要你填写一些信息，按提示填写。  

    ```
    package name: (demo) //上传包的名字不能与npm里面的包重复
    version: (1.0.0) 
    description: test npm input package
    entry point: (index.js) 
    test command: 
    git repository: 
    keywords: 
    author: 
    license: (ISC) 
    About to write to /Users/majunlong/VSCode/JS/npm/demo/package.json:

    {
        "name": "ncdhz-npm-demo-1",
        "version": "1.0.0",
        "description": "test npm input package",
        "main": "index.js",
        "scripts": {
        "test": "echo \"Error: no test specified\" && exit 1"
        },
        "author": "",
        "license": "ISC"
    }


    Is this OK? (yes) yes
    ```

5. 新建一个`index.js文件` `touch index.js`。
6. 添加一个累加函数

    ```js
    exports.sum=function(){
        let result = Array.from(arguments).reduce((prev,cur)=>{
            return prev + cur;
        });
        return result;
    }
    ```

    + 登入你的`npm` `npm login`

    ![截屏2020-03-3116.39.38](media/15856429111851/%E6%88%AA%E5%B1%8F2020-03-3116.39.38.png)

7. 需要在你邮箱中点击相应的链接。
8. 上传 `npm publish`
![截屏2020-03-3116.53.01](media/15856429111851/%E6%88%AA%E5%B1%8F2020-03-3116.53.01.png)

9. 如果你没有发布成功说明你的包名已经被占用。
10. 如果想要删除包，执行 `npm unpublish` 命令。
11. 查看包的版本信息 `npm view package_name versions`
