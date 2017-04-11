# CMake版本信息
cmake_minimum_required(VERSION 3.4.1)

# 支持-std=gnu++11
set(CMAKE_VERBOSE_MAKEFILE on)
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=gnu++11")

# 工程路径
set(pathToProject D:/android_studio_workspace/nftBookProj)
# Module 名称
set(moduleName nftBook)
# Native 路径
set(pathToNative ${pathToProject}/${moduleName}/src/main/cpp)

# 配置加载native依赖
include_directories(${pathToProject}/ARToolKit)
include_directories(${pathToProject}/ARToolKit/android)

# 添加待编译的文件
add_library(nftBookNative SHARED ${pathToNative}/ARMarkerNFT.c
                                 ${pathToNative}/ARMarkerNFT.h
                                 ${pathToNative}/color_convert_common.c
                                 ${pathToNative}/color_convert_common.h
                                 ${pathToNative}/nftBook.cpp
                                 ${pathToNative}/osgPlugins.h
                                 ${pathToNative}/trackingSub.c
                                 ${pathToNative}/trackingSub.h
                                 ${pathToNative}/VirtualEnvironment.c
                                 ${pathToNative}/VirtualEnvironment.h)

# 动态方式加载
add_library(so_libc++_shared SHARED IMPORTED)
# 引入so文件
set_target_properties(so_libc++_shared PROPERTIES IMPORTED_LOCATION ${pathToProject}/${moduleName}/src/main/jniLibs/${ANDROID_ABI}/libc++_shared.so)

# 动态方式加载
add_library(so_libnftBookNative SHARED IMPORTED)
# 引入so文件
set_target_properties(so_libnftBookNative PROPERTIES IMPORTED_LOCATION ${pathToProject}/${moduleName}/src/main/jniLibs/${ANDROID_ABI}/libnftBookNative.so)

target_link_libraries(nftBookNative
                      so_libc++_shared so_libnftBookNative
                      log GLESv1_CM z)