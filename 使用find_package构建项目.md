# 1.准备工作  
(1).将hello.h安装至/usr/local/include/hello下。代码如下：
```
#include<iostream>
using namespace  std;
void fun();
```
(2).将hello.cc做成共享库，安装至/usr/local/lib下。代码如下：
```
#include"hello.h"
void fun(){
    cout<<"hello world"<<endl;
}
```
(3).安装的CMakeLists.txt代码如下：
```
cmake_minimum_required(VERSION 3.4)
add_library(hello  SHARED hello.cc)
install(TARGETS hello DESTINATION lib)
install(FILES hello.h DESTINATION include/hello)
```
(4).新建t2目录，进入t2目录，新建cmake目录，在cmake目录下新建文件FindHELLO.cmake。在t2目录下新建main.cc,CMakeLists.txt。代码如下：
```
mkdir t2
cd t2
mkdir cmake
touch cmake/FindHELLO.cmake
touch main.cc
touch CMakeLists.txt
```
# 2.find_package作用  
(1).find_package语法:find_package(packagename)。
(2).个人理解：作用就是把名为packagename的.cmake文件找到，并加载到当前位置上。
(3).如何找到packagename? 通过设置CMAKE_MODULE_PATH。  
# 3.给出代码  
FindHELLO.cmake
```
find_path(HELLO_INCLUDE_DIR hello.h /usr/local/include/hello)
find_library(HELLO_LIBRARY NAMES hello PATH /usr/local/lib)
if(HELLO_INCLUDE_DIR AND HELLO_LIBRARY)
set(HELLO_FOUND TRUE)
endif()
```
CMakeLists.txt
```
cmake_minimum_required(VERSION 3.4)
project("myhello")
SET(CMAKE_MODULE_PATH ${PROJECT_SOURCE_DIR}/cmake)
find_package(HELLO)
if(HELLO_FOUND)
add_executable(hello main.cc)
include_directories(${HELLO_INCLUDE_DIR})
target_link_libraries(hello ${HELLO_LIBRARY})
endif()
```
main.cc
```
#include<hello.h>
int main(){
    fun();
}
```
