## CMake
### Quick Start
1. 编写CMakeLists.txt
    ```cmake
    # top-level CMakeLists.txt
    cmake_minimum_required(VERSION 3.10) # CMAKE版本
    project(diy_ds) # 项目名称

    # set用于测试参数
    set(CMAKE_CXX_STANDARD 17)  # 设置C++标准为C++17
    Set(CMAKE_CXX_STANDARD_REQUIRED True)

    # 增加子目录。子目录里也有CMakeLists.txt
    add_subdirectory(List)


    # List/CMakeLists.txt
    aux_source_directory(src SRC_LIST) # 列出目录里的所有源文件，并存到变量SRT_LIST里面
    include_directories(include) # 增加包含目录（相当于-I）
    library_directories(lib) # 增加库检索目录（相当于-L）

    add_executable(test ${SRC_LIST}) # 增加一个可执行文件生成目标
    # 为目标链接一个库
    # 注意链接库可以是一个前文的add_library target
    # 也可以是库文件的绝对路径
    # 也可以是库名称（相当于-l）
    target_link_libraries(test PUBLIC your_lib) 
    
    ```
2. 生成makefile(Unix Like)
   ```bash
   # 假设CMakeLists.txt放在顶级目录
   mkdir build
   cd build
   cmake ..
   ```
   若如此做，生成的文件都会在build目录里

   如果是Windows平台，默认生成VS解决方案
3. 构建项目
   直接`make`即可。

   注：同样可以`make clean` `make install`等

### 链接多线程库
一个跨平台的做法是：
```cmake
find_package(Threads)
target_link_libraries(your_target Threads::Threads)
```
由于Windows/Linux的多线程库是不一样的，CMake专门内置了一个package叫`Thread`，然后连接它即可

注意：**对多线程库的链接必须要在其他依赖多线程的库的前面！！**
