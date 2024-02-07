cmake_minimum_required(VERSION 3.0)

# Debug
set( CMAKE_BUILD_TYPE "Debug" )
set( CMAKE_CXX_FLAGS_DEBUG "-o0 -Wall -g -ggdb" )

#获取目录名称，控制器名称要求与目录相同
get_filename_component(PROJECT ${CMAKE_SOURCE_DIR} NAME)
project(${PROJECT})

#获取源代码
file(GLOB C_SOURCES ./src/*.c)
file(GLOB CPP_SOURCES ./src/*.cpp)
set(SOURCES ${C_SOURCES} ${CPP_SOURCES})

# webots libraries
link_directories($ENV{WEBOTS_HOME}/lib/controller)
set (LIBRARIES ${CMAKE_SHARED_LIBRARY_PREFIX}Controller${CMAKE_SHARED_LIBRARY_SUFFIX} ${CMAKE_SHARED_LIBRARY_PREFIX}CppController${CMAKE_SHARED_LIBRARY_SUFFIX})
include_directories(/Applications/Webots.app/include/controller/c /Applications/Webots.app/include/controller/cpp ./include /usr/local/include/eigen3)

# 编译
add_executable(${PROJECT} ${SOURCES})
# 链接webots库文件
target_link_libraries(${PROJECT} ${LIBRARIES})

# 将编译好的文件拷贝到工程目录以供webots检索
add_custom_command(TARGET ${PROJECT} POST_BUILD COMMAND ${CMAKE_COMMAND} -E
  copy ${CMAKE_BINARY_DIR}/${PROJECT} ${CMAKE_SOURCE_DIR})

