# yrm 和 nrm 的使用

1. `yrm` 是一个更改 `yarn` 源的一个工具
2. 下载 `yrm` `npm install -g yrm`
3. 查看当前使用的源`yrm ls`

    ```
    * npm ---- https://registry.npmjs.org/
      cnpm --- http://r.cnpmjs.org/
      taobao - https://registry.npm.taobao.org/
      nj ----- https://registry.nodejitsu.com/
      rednpm - http://registry.mirror.cqupt.edu.cn/
      npmMirror  https://skimdb.npmjs.com/registry/
      edunpm - http://registry.enpmjs.org/
      yarn --- https://registry.yarnpkg.com
    ```

4. 修改源 `yrm use cnpm`
5. 查看修改后的源 `yrm ls`

    ```
      npm ---- https://registry.npmjs.org/
    * cnpm --- http://r.cnpmjs.org/
      taobao - https://registry.npm.taobao.org/
      nj ----- https://registry.nodejitsu.com/
      rednpm - http://registry.mirror.cqupt.edu.cn/
      npmMirror  https://skimdb.npmjs.com/registry/
      edunpm - http://registry.enpmjs.org/
      yarn --- https://registry.yarnpkg.com
    ```

6. `nrm` 是一个更改 `npm` 源的一个工具
7. 下载 `nrm` `npm install -g nrm`
8. 查看当前使用的源`nrm ls`

    ```
    * npm ---- https://registry.npmjs.org/
      cnpm --- http://r.cnpmjs.org/
      taobao - https://registry.npm.taobao.org/
      nj ----- https://registry.nodejitsu.com/
      rednpm - http://registry.mirror.cqupt.edu.cn/
      npmMirror  https://skimdb.npmjs.com/registry/
      edunpm - http://registry.enpmjs.org/
      yarn --- https://registry.yarnpkg.com
    ```

9. 修改源 `nrm use cnpm`
10. 查看修改后的源 `nrm ls`

    ```
      npm ---- https://registry.npmjs.org/
    * cnpm --- http://r.cnpmjs.org/
      taobao - https://registry.npm.taobao.org/
      nj ----- https://registry.nodejitsu.com/
      rednpm - http://registry.mirror.cqupt.edu.cn/
      npmMirror  https://skimdb.npmjs.com/registry/
      edunpm - http://registry.enpmjs.org/
      yarn --- https://registry.yarnpkg.com
    ```

11. 设置`npm`源

    ```shell
    # 设置了淘宝源
    npm config set registry https://registry.npm.taobao.org
    ```
