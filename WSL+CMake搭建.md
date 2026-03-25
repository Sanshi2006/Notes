# 一.环境搭建

执行：

`sudo apt update`

`sudo apt install build-essential cmake gdb`

# 二.基本步骤
1.创建项目文件夹

`mkdir xxx`

然后创建文件：

`touch xxx`

2.在根目录下编写`CMakeLists.txt`，
必要格式如下：

```txt
cmake_minimumu_required(VERSION 3.10)


project(填项目名)

add_exectutable(项目名 main.cpp ……)

```

3.执行：
`mkdir build && cd build`

`cmake ..`

`make`

`.\项目名`

其中前两步仅需执行一遍 后续修改只需要执行后两步

# 三.细节补充
1.如果在文件夹下(`src`)有若干个子文件夹，其中有`.h`和`.cpp`文件
那么`CMakeLists.txt`上要加上：

`file(GLOB_RECURSE srcs "src/*.cpp")`
这种写法会递归查找src目录下所有以`.cpp`为后缀的文件，`srcs`是自定义的名字


然后
`add_executable(项目名 main.cpp ${srcs})`

要指明查找路径，比如说：
`target_include_directories(项目名 PRIVATE 子文件夹目录..)`

2.注意：如果你写的`.cpp`文件中全是泛型函数，那么`.cpp`不用写，全部放进`.h`文件

但在`CMakeLists.txt`中仍然要指明路径