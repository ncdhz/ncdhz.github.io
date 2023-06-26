# numpy的基础知识

1. 导入`numpy` `import numpy as np` (后面所有的操作都需要导入`numpy`)

2. 构建数组
    + `numpy` 中数组和 `python` 中`list` 最主要的区别是：`numpy`中数组的每一个元素的类型必须相同

    + 构建一个数组  

        ```python
        a = np.array([5, 10, 15, 20])
        ```

    + 向量和矩阵
        + `numpy` 的数组又可以叫矩阵或者向量
        + 一维的矩阵是向量  

            ```python
            构建一个向量（一维数组）
            vector = np.array([5, 10, 15, 20])
            构建一个矩阵（多维数组）
            matrix = np.array([[5, 10, 15],[20, 25, 30]])
            ```

    + 基本矩阵构建  

        ```python
        构建一个 0-24 的向量
        np.arange(24)  # 和python基本函数 range 差不多
        构建一个全 0 矩阵
        np.zeros((10,10)) # 参数为 shape 
        构建一个全 1 矩阵
        np.ones((10,10)) # 参数为 shape 
        构建一个指定值得矩阵
        np.full((10,10),'11') # 第一个参数为 shape 第二个参数为 val 
        构建一个单位矩阵
        np.eye(10) # 参数是行列数
        构建一个指定跨度和首元素结束元素的矩阵
        np.linspace(1,10,6,endpoint=False) # 第一个参数表示起始位置 第二个参数表示结束位置 第三个参数表示元素之间跨度 第四个参数表示创建的数组是否包含最后一个元素默认 True
        构建一个 3 * 5 矩阵，元素为 0-15
        np.arange(15).reshape(3,5) # reshape 表示把前面的向量变成 3 * 5 的矩阵
        构建一个随机矩阵 (矩阵中的元素大于 -1 小于 1)
        np.random.random((2,3)) # random参数是一个 shape
        构建一个首元素为 0 结束元素为 10 间隔相等的含有100个元素的向量
        np.linspace(0, 10, 100) # 第一个参数起始元素 第二个参数结束元素  第三个参数元素个数
        ```

    + 打印数组的形状，类型，维度，数量

        ```python
        a = np.array([5, 10, 15, 20.0])
        print(a.shape)
        print(a.dtype)
        print(a.ndim)
        print(a.size)
        ```

3. 数组中数据获取（数组中的行列下标是从0开始的）  

    ```python
    a = np.array([
        [1, 2, 3],
        [4, 5, 6],
        [7, 8, 9]
    ])
    通过下标获取
    a[1][0] # 表示 [4, 5, 6] 这列中的 4 这个元素
    获取 1-2 行的数据
    a[1:3] # 表示 1-2 行的数据其中 1 包含第一行 3 不包含第三行
    获取第第 1-2 列
    a[:,1:3] # 同上理 其中第一个 : 表示选择所有行
    获取第第 1-2 行
    a[1:3,:] # 同上
    指定开始下标和结束下标和跨度获取元素
    a[0:1:1] # 第一个参数指定开始行（包含开始行） 第二个参数指定结束行 （不包含结束行） 第三个参数表示获取元素的跨度
    ```

4. 数组中元素的基本处理
    + 对比  

        ```python
        a = np.array([
        [1, 2, 3],
        [4, 5, 6],
        [7, 8, 9]
        ])
        比较a中元素等于 7 的
        a == 7 # 会返回一个和原数组一样的 bool 类型的数组
        返回值: [[False False False]
        [False False False]
        [ True False False]]

        同理可以比较 >, <, <=, >= 等，返回结果依然是一个和元素组维度一模一样的 bool 类型的数组

        找到 a > 7 的元素
        aa = a > 7
        print(a[aa]) # 获取到 a > 7 的 bool 数组，通过索引拿到 a>7 数组中的值

        & 和 | 符号的使用
        aa = (a > 7) & (a < 9)
        print(a[aa]) # 获取 a > 7 并且 a < 9 的元素
        aa = (a > 7) | (a <3 )
        print(a[aa]) # 获取 a > 7 或者 a < 3 的元素
        ```

    + 类型转换  

        ```python
        b = np.array(['1','2','3']) # 数组为字符串类型
        print(b.dtype) 
        b = b.astype(float) # 数组转换为 float 类型
        print(b.dtype)
        ```

    + 最大值，最小值，求和等  

        ```python
        a = np.array([
            [1, 2, 3],
            [4, 5, 6],
            [7, 8, 9]
            ])
        a.min() # 最小值 a.min(axis=1) 按行求最小值 a.min(axis=0) 按列求最小值 a[:,1].min() a中第一列的最小值
        a.argmin() # 返回最小值序号 同上
        a.max() # 最大值 同上
        a.argmax() # 返回最大值序号 同上
        a.sum() # 求和 同上
        ```

    + 加减乘除  

        ```python
        a = np.array([10, 20, 30, 40])
        b = np.arange(4)
        a + b # 对应位置元素相加 [10, 21, 32, 43]
        a - b # 对应位置元素相减 [10, 19, 28, 37]
        a * b # 对应位置元素相乘 [0, 20, 60, 120]
        b / a # 对应位置元素相除 [0., 0.05, 0.06666667, 0.075]
        a ** b # 对应位置元素进行** [1, 20, 900, 64000]
        a + 1 # 每个元素都加 1 [11, 21, 31, 41]
        a - 1 # 每个元素都减 1 [ 9, 19, 29, 39]
        a * 2 # 每个元素都乘 2 [20, 40, 60, 80]
        a / 2 # 每个元素都除 2 [5., 10., 15., 20.]
        a ** 2 # 每个元素的平方 [100,  400,  900, 1600]
        c = np.arange(4).reshape((2,2))
        d = np.ones((2,2),dtype=np.int)
        d.dot(c) # 以矩阵乘法计算
        np.dot(d,c) # 和上面一样
        ```

5. 基本函数

    ```python
    a = np.arange(3)
    np.exp(a) # 以 e 为底的 ** 操作  [1., 2.71828183, 7.3890561]
    np.sqrt(a) # 对 a 中每个元素开方 [0., 1., 1.41421356]
    np.floor(10 * np.random.random((3,))) # 向下取整 [6., 6., 8.]
    np.title(a, (2, 2)) # 对a进行扩展行变成原来的两倍列也变成原来的两倍

    b = np.arange(15).reshape(3, 5)
    c = b.ravel() # 矩阵变向量
    b.T # 转置
    c.shape = (5, 3) # 和 reshape(3, 5) 一样

    d = np.floor(10 * np.random.random((2, 2)))
    e = np.floor(10 * np.random.random((2, 2)))
    np.vstack((d, e)) # 按行拼接d和e
    np.hstack((d, e)) # 按列拼接d和e

    f = np.floor(10 * np.random.random((12, 2)))
    np.vsplit(f, 3) # 按行平均切成三份
    np.vsplit(f, (3, 4)) # 指定切割位置进行切割 3 这个地方切一下 4 这个地方切一下

    f = np.floor(10 * np.random.random((2, 12)))
    np.hsplit(f, 3)
    np.hsplit(f, (3, 4)) # 同上按列切分

    浅拷贝和深拷贝
    g = f # 是浅拷贝 id(g) == id(f)
    g = f.view() # 是视图 id(g) != id(f) 但是操作f，g也会发生变化
    g = f.copy() # 是深拷贝 id(g) != id(f) 且f，g相互独立

    排序
    h = np.array([3, 6, 0, 10, 19, 4])
    np.sort(h) # 返回排好序的数组
    np.argsort(h) # 返回排好序的数组下标
    ```
