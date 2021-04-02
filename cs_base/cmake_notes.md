**Cmake 资料**

[cmake学习笔记之add_library、target_link_libraries和link_directories](https://blog.csdn.net/bigdog_1027/article/details/79113342)

[link_directories, LINK_LIBRARIES, target_link_libraries使用总结](https://blog.csdn.net/arackethis/article/details/43488177)

[Clion 结合主流开发库的cmakelist完整配置](https://www.codeqq.com/log/MyopB5gZ.html)

[cmake 添加头文件目录，链接动态、静态库](https://www.cnblogs.com/binbinjx/p/5626916.html)

[vscode利用cmake调试](https://blog.csdn.net/code_segment/article/details/81151443)

[18.find-exec — 执行find的结果](https://blog.csdn.net/KingBoyWorld/article/details/78298559)

[cmake list 常用变量](https://blog.csdn.net/gubenpeiyuan/article/details/8667279)


note参考：  https://github.com/ttroy50/cmake-examples

### cmake 基础

#### 运行

CMakeLists.txt 是 cmake 命令

    cmake_minimum_required(VERSION 3.5)
    project (hello_cmake)
    add_executable(hello_cmake main.cpp)
   
 命令：
 
    mkdir build
    cd build && cmake ../
    make && ../hello_cmake
 
     建立build的目录的原因是，cmake会生成很多中间的临时文件，避免污染源码目录
    ../ 表示在上一级目录中，查找CMakeLists.txt（文件名是固定，不可修改）文件。
 
#### 内置变量

CMake的关于文件的内置变量用于屏蔽各个编译地址的差异，可以根据注释酌情使用

    Variable	                       Info

    CMAKE_SOURCE_DIR                 The root source directory
    CMAKE_CURRENT_SOURCE_DIR	     The current source directory if using sub-projects and directories.
    PROJECT_SOURCE_DIR	             The source directory of the current cmake project.
    CMAKE_BINARY_DIR	             The root binary / build directory. This is the directory where you ran the cmake command.
    CMAKE_CURRENT_BINARY_DIR	     The build directory you are currently in.
    PROJECT_BINARY_DIR	             The build directory for the current project.

#### 链接库和链接头文件

相当于给GCC添加 -I 选项

    target_include_directories(${PROJECT_NAME} PRIVATE
        ${PROJECT_SOURCE_DIR}/include
    )
    
生成静态库和使用静态库

    add_library(hello_library STATIC
        src/Hello.cpp
    )
    target_link_libraries( hello_binary PRIVATE
        hello_library
    )

生成 libhello_library.so 只需要将修饰符变为 SHARED即可，其它不用变

    add_library(hello_library SHARED
        src/Hello.cpp
    )
    add_library(hello::library ALIAS hello_library)
    target_link_libraries( hello_binary PRIVATE
        hello::library
    )

    修饰符	        含义
    INTERFACE	the directory is added to the include directories for any targets that link this library.
    PRIVATE	the directory is added to this target’s include directories
    PUBLIC	As above, it is included in this library and also any targets that link this library.





    
    


