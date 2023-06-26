# pip 豆瓣源

1. 一招解决超时问题

    ```shell
    pip --default-timeout=100 install -r ./requirements.txt -i http://pypi.douban.com/simple/ --trusted-host pypi.douban.com 
    ```
