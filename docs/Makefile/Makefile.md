# Makefile

我将通过一个例子来讲解 `makefile` 文件

## 初始化项目

1. 新建文件 `sub.c`

    ```c
    double reduce(double x, double y) {
        return x - y;
    }
    ```

2. 新建文件 `sum.c`

    ```c
    double add(double x, double y) {
        return x + y;
    }
    ```

3. 新建文件 `math.h`

    ```h
    double reduce(double x, double y);
    double add(double x, double y);
    ```

4. 新建文件 `main.c`

    ```c
    #include <stdio.h>
    #include "math.h"
    
    int main(int argc, char const *argv[]) {
        printf("213.321 + 312.32 = %lf\n", add(213.321, 312.32));
        printf("321.321 - 112.12 = %lf\n", reduce(321.321, 112.12));
        return 0;
    }

    ```

大家看依赖关系应该可以很明显的知道最后 `main` 会使用到 `sub.c` `sum.c` 中的函数

## 第一版 Makefile

1. 新建 `makefile`
2. 添加如下配置

        ```makefile
        main:main.o sub.o sum.o
            gcc main.o sub.o sum.o -o main
        main.o:main.c
            gcc -c main.c -o main.o
        sub.o:sub.c
            gcc -c sub.c -o sub.o
        sum.o:sum.c
            gcc -c sum.c -o sum.o
        clean:
            rm -rf *.o main out.a
        ```

3. 解析
    + `makefile` 规则

        ```
        target:prerequisite
            command
        # 我们可以对比上面的配置，是不是很明显的可以看出 target 是目标文件，prerequisite 是生成这个目标所需要的文件，command 是执行的命令（也就是执行 make target 时所需要执行的命令）
        ```

    + 上面代码的含义

        ```shell
        # 生成 main 文件需要 main.o sub.o sum.o 文件并要执行 gcc main.o sub.o sum.o -o main 这条命令 （显然当执行 make main[可以省略main] 是会先执行下面的一个个规则）
        main:main.o sub.o sum.o
            gcc main.o sub.o sum.o -o main
        # 生成 main.o 文件需要 main.c 文件并要执行 gcc -c main.c -o main.o 这条命令
        main.o:main.c
            gcc -c main.c -o main.o
        # 生成 sub.o 文件需要 sub.c 文件并要执行 gcc -c sub.c -o sub.o 这条命令
        sub.o:sub.c
            gcc -c sub.c -o sub.o
        # 生成 sum.o 文件需要 sum.c 文件并要执行 gcc -c sum.c -o sum.o 这条命令
        sum.o:sum.c
            gcc -c sum.c -o sum.o
        # 通过执行 make clean 会执行 rm -rf *.o main out.a 这条命令
        clean:
            rm -rf *.o main a.out
        ```

4. 在当前目录打开终端执行 `make` 就可以看见各种文件的输出了。如果要删除生成的文件执行 `make clean`

## 第二版 Makefile （变量）

1. 首先我们需要知道的是 makefile 文件中的变量是怎么定义的，和怎么使用的

    ```
    # 定义
    变量名=内容
    # 注意 = 左右两边不能有空格
    # 使用
    $(变量名) or ${变量名}
    ```

2. 新建 `makefile` 添加如下配置

    ```shell
    # 下面全是变量
    # 定义此变量是为了方便更换编译器
    CC=gcc
    # 指定的文件名
    obj=main
    obj1=sub
    obj2=sum
    OBJ=main.o sub.o sum.o
    # 下面通过变量的获取方式做了简单的替换
    # 可以参考第一版 makefile 配置
    $(obj):$(OBJ)
        $(CC) $(OBJ) -o $(obj)
    $(obj).o:$(obj).c
        $(CC) -c $(obj).c -o $(obj).o
    $(obj1).o:$(obj1).c
        $(CC) -c $(obj1).c -o $(obj1).o
    $(obj2).o:$(obj2).c
        $(CC) -c $(obj2).c -o $(obj2).o
    clean:
        rm -rf *.o $(obj) a.out
    ```

3. 不难理解和第一版的makefile对比，第二版的makefile并只是做了一些简单的变量替换，使用方式也和第一没有区别

## 第三版 Makefile （隐含规则）

1. 首先我们要知道什么是隐含规则，通俗解释就是 `makefile` 文件中没有书写任何配置时就已经存在的变量
2. 下面是一些 `makefile` 的隐含规则

    ```
    AR      库文件维护程序的名称，默认值为ar
    AS      汇编程序的名称，默认值为as
    CC      C编译器的名称，默认值为cc
    CPP     C预编译器的名称，默认值为$(CC) –E
    CXX     C++编译器的名称，默认值为g++
    FC      FORTRAN编译器的名称，默认值为f77
    RM      文件删除程序的名称，默认值为rm –f
    ARFLAGS 库文件维护程序的选项，无默认值
    ASFLAGS 汇编程序的选项，无默认值
    CFLAGS  C编译器的选项，无默认值
    CPPFLAGS C预编译的选项，无默认值
    CXXFLAGS C++编译器的选项，无默认值
    FFLAGS  FORTRAN编译器的选项，无默认值
    $@      目标集合
    $%      当目标是函数库文件时, 表示其中的目标文件名
    $<      第一个依赖目标. 如果依赖目标是多个, 逐个表示依赖目标
    $?      比目标新的依赖目标的集合
    $^      所有依赖目标的集合, 会去除重复的依赖目标
    $+      所有依赖目标的集合, 不会去除重复的依赖目标
    $*      这个是GNU make特有的, 其它的make不一定支持
    ```

3. 新建 `makefile` 添加下面配置

    ```shell
    CC=gcc
    obj=main
    obj1=sub
    obj2=sum
    OBJ=main.o sub.o sum.o
    # 生成警告和调试信息
    CFLAGS=-Wall -g
    $(obj):$(OBJ)
        # $^ 表示 $(OBJ)也就是所有依赖 $@ 表示的是 $(obj)也就是目标依赖
        $(CC) $(CFLAGS) $^ -o $@
    $(obj).o:$(obj).c
        # $< 表示 $(obj).c 也就是第一个依赖目标
        $(CC) $(CFLAGS) -c $< -o $@
    $(obj1).o:$(obj1).c
        $(CC) $(CFLAGS) -c $< -o $@
    $(obj2).o:$(obj2).c
        $(CC) $(CFLAGS) -c $< -o $@
    clean:
        rm -rf *.o $(obj) a.out
    ```

4. 通过观看上面的 `makefile` 的配置代码我们可以发现其中增加了一些符号，这些符号我们可以理解成 `makefile` 为了方便书写自带的一些全局变量

## 第四版 Makefile （通配符）

1. 新建 `makefile` 添加如下配置

    ```shell
    CC=gcc
    obj=main
    OBJ=main.o sub.o sum.o
    $(obj):$(OBJ)
        $(CC) $^ -o $@
    # 表示每一个 .c 文件单独作为一个依赖生成一个 .o 文件 
    # 对比上面配置也就知道了这两行配置做了一些什么事了
    %*.o:%*.c
        $(CC) $< -o $@
    clean:
        rm -rf *.o $(obj) a.out
    ```

2. 其实可以很明显的看出其中的不同也就是使用了一个通配符代替了上面的多行配置
