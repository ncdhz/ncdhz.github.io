# jekyll-TeXt-theme 简述

1. 到这里的小伙伴应该都具有一定的[Jekyll](/2019/04/26/jekyll.html)的基础了，废话不说我们进入正题。
2. 首先我们进入[jekyll-TeXt-theme](https://github.com/kitian616/jekyll-TeXt-theme)的`github`地址下载一下[jekyll-TeXt-theme](https://github.com/kitian616/jekyll-TeXt-theme)的压缩包
3. 运行 `jekyll-TeXt-theme`  

    ```shell
    cd jekyll-TeXt-theme-master
    sudo bundle  install // 下载依赖
    bundle exec jekyll serve
    ```  

4. 简述配置 _config.yml
    + `text_skin: 主题`
    + `highlight_theme: 主题亮度`
    + `url: 网站协议或域名`
    + `baseurl: 网站根路径`
    + `title: 网站标题`
    + `description: 网站简述`

    修改配置要重启服务 `bundle exec jekyll serve`

    修改配置后的页面

    + `lang: 语言`
    + `timezone: 时间`
    + `author: 作者基本信息`
    + `repository: github源码库`
    + `license: 开原协议`

        ```shell
        # Markdown 增强
        # Mathjax
        mathjax: true
        mathjax_autoNumber: true
        # Mermaid
        mermaid: true
        # Chart
        chart: true
        ```

    + `paginate: 分页(8)`
    + `sharing: 分享（"addtoany", "addthis"）`
    + `comments: 评论需要注册相应的号然后填写对应信息`  

    第一种方案：注册 `Disqus` 申请 `shortname`  

        ```
        comments:
            provider: disqus
            disqus:
                shortname: "your-disqus-shortname"
        ```  

    第二种方案：注册 [gitalk](https://github.com/settings/applications/new)

        ```
        comments:
            provider: gitalk
            gitalk:
                clientID    : "github-application-client-id"
                clientSecret: "github-application-client-secret"
                repository  : 你绑定项目的repository 不是comments的repository
                owner       : 你的github的用户名
                admin: 你的github的用户名
        ```  

    第三种方案：使用下面点击量功能的操作获取相关信息  

        ```
        comments:
            provider: valine
            valine:
                app_id  : "your-leanCloud-app-id"
                app_key : "your-leanCloud-app-key"
        ```

    + `pageview: 点击量功能`
        + 注册 [LeanCloud](https://leancloud.cn)  
        + 访问`控制台` 注册账号  
        + 点击`创建应用`根据实力选择计价模式  
        + 创建`Class`  

        ```
        pageview:
        provider: leancloud
        leancloud:
            app_id    : "your-leanCloud-app-id"
            app_key   : "your-leanCloud-app-key"
            app_class : "your-leanCloud-app-class"
        ```

    + 基本配置如上如果需要更详细的参考资料移步 [jekyll-TeXt-theme](https://tianqi.name/jekyll-TeXt-theme/docs/zh/configuration)

5. 撰写博客
    + `cd _posts` 目录
    + 撰写博客和 [Jekyll](/2019/12/03/jekyll-简述/> ) 的编写方式和新建方式一样
    + `jekyll-TeXt-theme` 新增了很多有用的参数配置，请移步 [撰写博客](https://tianqi.name/jekyll-TeXt-theme/docs/zh/writing-posts) [布局](https://tianqi.name/jekyll-TeXt-theme/docs/zh/layouts)
6. `jekyll-TeXt-theme` 这个主题还有很多别的配置这里就不一一列举了大家可以参考官方文档 [jekyll-TeXt-theme](https://tianqi.name/jekyll-TeXt-theme/docs/zh/quick-start)
