# 简谈 Promise

1. 通过使用 `Promise` 可以有效的避免 `回调地狱`
2. `回调地狱`例子

    + 存在问题的例子

        ```js
        /**
        * @param {更新数据库的sql} sql 
        * @param {更新数据库所要的时间} time 
        */
        function updateDatabase(sql, time) {
            setTimeout(() => {
                console.log('update success ' + sql)
            }, time);
        }
        updateDatabase('sql 1', 400)
        updateDatabase('sql 2', 300)
        updateDatabase('sql 3', 200)
        // 当执行完上面的三条更新语句我们会得到如下的输出
        // update success sql 3
        // update success sql 2
        // update success sql 1
        // 最后一条更新语句在最前面输出了，如果这种情况在实际项目中发生可能会出现数据错误等问题 所以就有了下面的一种解决方案
        ```

    + 通过回调解决上面问题

        ```js
        function updateDatabase(sql, time, callback) {
            setTimeout(() => {
                callback(sql)
            }, time);
        }
        updateDatabase('sql 1', 400, (sql)=>{
            console.log('update success ' + sql)
            updateDatabase('sql 2', 300, (sql) => {
                console.log('update success ' + sql)
                updateDatabase('sql 3', 200, (sql) => {
                    console.log('update success ' + sql)
                })
            })
        })
        // 输出结果如下所示
        // update success sql 1
        // update success sql 2
        // update success sql 3
        // 显然通过这种方式我们已经解决好了问题 不过我们会发现 如果有很多次跟新操作那么代码将一层层嵌套下去变得难以维护
        ```

3. 通过 `Promise` 解决 `回调地狱`
    + 老式解决方案

        ```js
        function updateDatabase(sql, time) {
            return new Promise((resolve, reject) => {
                setTimeout(() => {
                    if (resolve) {
                        // 成功时执行
                        // 通过 .then 执行
                        resolve(sql)
                    } else {
                        // 失败时执行
                        // 通过 .catch 执行
                        reject(sql)
                    }
                }, time);
            })   
        }
        
        updateDatabase('sql 1', 400).then((sql) => {
            console.log('update success ' + sql)
            return updateDatabase('sql 2', 300)
        }).then((sql) => {
            console.log('update success ' + sql)
            return updateDatabase('sql 3', 200)
        }).then((sql) => {
            console.log('update success ' + sql)
        })
        // 结果
        // update success sql 1
        // update success sql 2
        // update success sql 3
        // 我们可以观察上面的调用代码发现这就是一种链式调用 
        // 也就是说不论你有多少条更新代码最终形成的是一条线性代码 不会出现一直回调的结果
        ```

    + 最新处理方案

        ```js
        function updateDatabase(sql, time) {
            return new Promise((resolve, reject) => {
                setTimeout(() => {
                    if (resolve) {
                        // 成功时执行
                        resolve(sql)
                    } else {
                        // 失败时执行
                        reject(sql)
                    }
                }, time);
            })   
        }
        async function updateAllDatabase() {
            const sql1 = await updateDatabase('sql 1', 400)
            const sql2 = await updateDatabase('sql 2', 300)
            const sql3 = await updateDatabase('sql 3', 200)
            console.log('update success ' + sql1)
            console.log('update success ' + sql2)
            console.log('update success ' + sql3)
        }
        updateAllDatabase()
        // 结果
        // update success sql 1
        // update success sql 2
        // update success sql 3
        // 通过上面的代码 我们可以对比第一个存在问题的代码 是不是很像 好像又回归到了原始  只不过顺便解决了问题
        ```

4. `Promise` 基本使用规范
    + `await` 必须在 `async` 修饰的函数中
    + `Promise` 传入的是一个函数 有两个参数（都是方法） 第一个：运行成功时执行 第二个：运行失败时执行
