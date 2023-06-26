# 配置 Jupyter 远程访问

1. 生成密码

    ```python
    # 在命令行中输入 python
    # 接着输入如下命令 
    from notebook.auth import passwd
    passwd()
    # 这里会要你输入密码
    # 接下来你会得到一个字符串（就是加密好的密码）
    ```

2. 生成配置文件

    ```shell
    jupyter notebook --generate-config
    ```

3. 修改配置文件

    ```shell
    # 进入配置文件
    vi ~/.jupyter/jupyter_notebook_config.py
    # 做下面的修改
    c.NotebookApp.allow_remote_access = True
    c.NotebookApp.open_browser = False
    c.NotebookApp.ip='*'
    c.NotebookApp.password = '刚才生成的密码'
    ```

4. 配置工作目录 （不是必须）

    ```shell
    c.NotebookApp.notebook_dir='你工作文件夹目录'
    ```

5. 启动 `jupyter`

    ```shell
    jupyter notebook
    ```

    ```shell
    # 后台启动
    nohup jupyter notebook --allow-root > jupyter.log 2>&1 &
    # jupyter.log 这个是启动日志存储文件（你可以找一个合适的地方启动 jupyter）
    ```
