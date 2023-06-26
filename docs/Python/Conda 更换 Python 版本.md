# Conda 更换 Python 版本

1. 下载需要的 `python` 版本

    ```shell
    conda create -n name python=xx
    如：
    conda create -n python35 python=3.5
    ```

2. 通过此命令可以查看本机存在的python版本

    ```shell
    conda info --envs
    ```

3. 激活刚才新建的`python`版本

    ```shell
    conda activate name
    如：
    conda activate python35
    查看当前python版本
    python --version
    ```

4. 删除 `python` 版本

    ```shell
    conda remove -n python35 --all
    ```