[TOC]

# 2、静态库与动态库

- 定义：一类文件
- 分类
- 动态库与静态库
- 库的优点：1.代码保密   2.方便部署和分发

## 2.1 静态库的制作

- 命名规则：

   - Linux：libxxx.a

     ​	lib: 前缀

     ​	xxx：名字

     ​	.a : 后缀

  - Windos： libxxx.lib

- 静态库的制作：
   - gcc获得.o文件
   - 将.o文件打包。使用ar工具

- 一个例子

静态库 libcalc.a 的制作

  1. 把文件分类打包好（便于管理），如图：

     ![2021-06-10 10-59-32 的屏幕截图 (1)](D:\solft\2021-06-10 10-59-32 的屏幕截图 (1).png)

     []: 

     

2. 使用 ```gcc/g++ -c```生成 .o 文件。

```bash
cd src
gcc/g++ -c add.c subtract.c -I ../include 
```

3. 使用 ```ar```命令打包

```bash
ar rcs rcs libcalc.a add.o subtruct.o
mv libcalc.a ../lib
```

4. 编译

```bash
gcc/g++ main.c -o app -I ./include -l calc -L /lib
```

## 2.2 动态库

- 命名规则

  - linux：libxxx.so

    ​	lib : 前缀（固定）

    ​	xxx ：库的名字，自己起

    ​	.so : 后缀（固定）
  
  - windows : libxxx.dll

- 动态库的制作

  - gcc 得到 .o 文件,得到和位姿无关的代码

  ```bash
  gcc -c -fpic/-FPIC a.c b.c
  ```

  - gcc 得到动态库

  ```bash
  gcc -shared a.o b.o -o libcalc.so
  ```

## 2.3 工作原理

- 静态库:gcc进行链接时,会把静态库代码打包到可执行程序中
- 动态库:gcc进行链接时,动态库的代码不会被打包到可执行程序中
- 程序启动后,动态库会被动态加载到内存中,通过 ldd (list dynamic dependencies)命令检查动态库依赖关系
- 如何定位共享库?  1. 环境变量 LD_LIBRARY_PATH 

## 2.4 静态库与动态库的对比

### 2.4.1 静态库的优缺点

优点：

- 静态库被打包到应用程序中加载速度快

- 发布程序无需提供静态库，移植方便

缺点：

- 消耗系统资源，浪费部署内存
- 更新、部署、发布麻烦

### 2.4.2 动态库的优缺点

优点：

- 可实现进程间资源共享（共享库）
- 更新、部署、发布简单
- 可以控制何时加载动态库

缺点：

- 加载速度比静态库慢
- 发布程序时需要提供依赖的动态库

