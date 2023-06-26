# pandas的基础知识

1. 导入`numpy` `import numpy as np` (后面部分操作需要用到`numpy`，如果对`numpy`不了解,移步[numpy的基本使用](/2020/03/30/numpy-de-ji-chu-zhi-shi/))

2. 导入`pandas` `import pandas as pd` (后面所有的操作都需要导入`pandas`)

3. 构建`Series`

    + 一个空的`Series`

        ```python
        p = pd.Series()
        输出
        Series([], dtype: float64)
        ```

    + 含有数据的`Series`

        ```python
        p = pd.Series([312, 4, 3, 64, 12, 543])
        输出
        0    312
        1      4
        2      3
        3     64
        4     12
        5    543
        dtype: int64
        ```

    + 含有数据指定索引的`Series`

        ```python
        p = pd.Series([312,4,3,64,12,543], index=["qwe","ewq","ewq","qwe","qwe","ewq"])
        输出
        qwe    312
        ewq      4
        ewq      3
        qwe     64
        qwe     12
        ewq    543
        dtype: int64
        ```

    + 指定数据类型

        ```python
        p = pd.Series(dtype=np.int)
        输出
        Series([], dtype: int64)
        ```

4. `Series` 基本操作
    + 初始化 `Series`  

        ```python
        p = pd.Series(np.floor(10*np.random.random(6)), index=["qwe","ewq","ewq","qwe","qwe","ewq"],dtype=np.int32)
        ```

    + 索引

        ```python
        p["qwe"] # 通过指定的 index 的英文索引获取
        p.loc["qwe"] # 同上
        p[1] # 直接通过序号索引获取
        p.iloc[1] # 同上
        p[1:3] # 包含 1 号位，不包含 3 号位
        p.iloc[1:3] # 同上
        p[:3] # 从头开始到 3 截止不包含 3
        p.iloc[:3] # 同上
        ```

    + 基本函数也包含`numpy`的一些基本函数 可以移步 [numpy](/2020/03/30/numpy-de-ji-chu-zhi-shi/#basicFunctions)  

        ```python
        p.head(2) # 显示前两条数据 参数：条数 默认 5 条
        p.tail(2) # 同上只是显示后两条

        新建 p1 = pd.Series([1,32,123,np.nan,321,312])
        p1.isnull() # 是 nan 显示为 True
        输出
        0    False
        1    False
        2    False
        3     True
        4    False
        5    False
        dtype: bool

        p1.notnull() # 结果和上面相反
        ```

    + 基本运算和`numpy`差不多 可以移步 [numpy](/2020/03/30/numpy-de-ji-chu-zhi-shi/#dataProcessing)
5. 构建`DataFrame`
    + 首先我们应该知道 `Series` 是 `DataFrame` 的子集
    + 构建一个空的 `DataFrame`

    ```python
      df = pd.DataFrame()
      输出
      Empty DataFrame
      Columns: []
      Index: []
    ```

    + 含有数据的`DataFrame`

    ```python
      df = pd.DataFrame([5, 4, 3, 2, 1])
      输出
         0
      0  5
      1  4
      2  3
      3  2
      4  1
    ```

    + 指定元素名字和元素类型

    ```python
      df = pd.DataFrame([5, 4, 3, 2, 1], columns=["frist"], dtype=np.float)
         frist
      0    5.0
      1    4.0
      2    3.0
      3    2.0
      4    1.0
    ```

    + 从文件中读取
        + 从`csv`文件中读取 `np.read_csv("xx.csv")`
        + 从`excel`文件中读取 `pd.read_excel("xx.xlsx")` (需要安装`xlrd`库)
        + 从`json`文件中读取 `pd.read_json("xx.json")`
        + 从`sql`中获取数据  

            ```python
            db = pymysql.connect('host', 'user', 'password', 'database')
            pd.read_sql("sql语句", con=db)
            ```

6. `DataFrame` 基本操作

    ```py
    # DataFrame 的基本操作可以说和 Series 一样，(有可能含有部分差别)。
    # 因为 Series 是 DataFrame 的子集，我们操作时通常是获取 Series 然后在进行数据处理的，详情可以看上面 Series 的基本操作。
    ```

7. 最后

    ```py
    # pandas 和 numpy 的兼容性很强，可以说 numpy 里面的很多函数在 pandas 中都是可以用的，甚至 pandas 和 numpy 可以直接进行运算。
    ```
