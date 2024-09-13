cmake_minimum_required(VERSION 2.8)

set(PROJECT mxh)

project(${PROJECT})

set(CMAKE_CXX_STANDARD_REQUIRED true)
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -Wall -Werror")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall -Werror")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -DTHREADED")

set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${PROJECT_SOURCE_DIR}/libs)
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${PROJECT_SOURCE_DIR}/libs)
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${PROJECT_SOURCE_DIR}/build)

# c++ 标准
if(NOT CMAKE_CXX_STANDARD)
    set(CMAKE_CXX_STANDARD 17)
endif()
message(STATUS "-std = c++${CMAKE_CXX_STANDARD}")

# 设置默认的编译级别为 realse
if(NOT CMAKE_BUILD_TYPE)
    set(CMAKE_BUILD_TYPE Release CACHE STRING "Build type" FORCE)
    set(CMAKE_CXX_FLAGS_RELEASE "-g")
endif()
message(STATUS "Build type: ${CMAKE_BUILD_TYPE}")

# 设置运行时路径
set(CMAKE_CXX_FLAGS_DEBUG "$ENV{CXXFLAGS} -O0 -Wall -g -ggdb -Wl,-rpath=./libs")
set(CMAKE_CXX_FLAGS_RELEASE "$ENV{CXXFLAGS} -O3 -Wl,-rpath=./libs")

# 设置头文件包含路径
include_directories(${PROJECT_SOURCE_DIR}/include)

# 第三方库路径
include_directories(${PROJECT_SOURCE_DIR}/thirdparty/include)
link_directories(${PROJECT_SOURCE_DIR}/thirdparty/libs)

# 增加预编译命令
add_compile_definitions(SPDLOG_COMPILED_LIB)

# 增加子项，本项目的 .so 
# add_subdirectory(${PROJECT_SOURCE_DIR}/src/common)
# add_subdirectory(${PROJECT_SOURCE_DIR}/src/some)

# 设置链接库
set(THIRDPART )
set(LIBRARY_COMMON )
set(LIBRARY_PRIVATE )

add_executable(${PROJECT} src/main.cpp)

target_link_libraries(${PROJECT} ${THIRDPART} ${LIBRARY_COMMON} ${LIBRARY_PRIVATE})

# 设置安装路径
install(TARGETS ${PROJECT} 
        RUNTIME DESTINATION ${PROJECT_SOURCE_DIR}/bin/
        ARCHIVE DESTINATION ${PROJECT_SOURCE_DIR}/bin/libs/
        LIBRARY DESTINATION ${PROJECT_SOURCE_DIR}/bin/libs/
)

# 设置安装移动第三方库文件
install(DIRECTORY ${PROJECT_SOURCE_DIR}/thirdparty/libs/ ${PROJECT_SOURCE_DIR}/libs/ 
        DESTINATION ${PROJECT_SOURCE_DIR}/bin/libs/
)
