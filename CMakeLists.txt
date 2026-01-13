# Works with 3.11 and tested through 3.21
cmake_minimum_required(VERSION 3.11 FATAL_ERROR)

set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

project(
  hydrox
  VERSION 0.1
  DESCRIPTION "An great project by uncledrew0113"
  LANGUAGES C CXX)
  
# C++ standard
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

if (NOT CMAKE_BUILD_TYPE)
    set(CMAKE_BUILD_TYPE Release)
endif()

message(STATUS "Build type: ${CMAKE_BUILD_TYPE}")

if(MSVC)
  add_compile_options(/W4)  # 警告级别4
  # add_compile_options(/WX)  # 可选：将警告视为错误
  if(CMAKE_BUILD_TYPE MATCHES "Debug")
    add_compile_options(/Od /Zi)  # 关闭优化，生成调试信息
  else()
    add_compile_options(/O2)  # 优化
  endif()
else()
  add_compile_options(-Wall -Wextra -Wpedantic)
  # add_compile_options(-Werror)  # 可选：将警告视为错误
  if(CMAKE_BUILD_TYPE MATCHES "Debug")
    add_compile_options(-O0 -g)
  else()
    add_compile_options(-O2)
  endif()
endif()

# ---------- Output directories (per-config aware) ----------
if(NOT DEFINED PROJECT_OUTPUT_DIR)
    set(PROJECT_OUTPUT_DIR ${CMAKE_BINARY_DIR}/${CMAKE_SYSTEM_PROCESSOR})
endif()

message(STATUS "CMAKE_BINARY_DIR = ${CMAKE_BINARY_DIR}")
message(STATUS "CMAKE_SYSTEM_PROCESSOR = ${CMAKE_SYSTEM_PROCESSOR}")
message(STATUS "PROJECT_OUTPUT_DIR = ${PROJECT_OUTPUT_DIR}")

set(PROJECT_INCLUDE_DIR ${PROJECT_OUTPUT_DIR}/include)

# 创建输出目录
file(MAKE_DIRECTORY ${PROJECT_INCLUDE_DIR})
file(MAKE_DIRECTORY ${PROJECT_OUTPUT_DIR}/bin)
file(MAKE_DIRECTORY ${PROJECT_OUTPUT_DIR}/lib)

# 设置目标输出路径
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${PROJECT_OUTPUT_DIR}/bin)
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${PROJECT_OUTPUT_DIR}/lib)
set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${PROJECT_OUTPUT_DIR}/lib)

# Multi-config generators (Visual Studio, Xcode) support
# foreach(OUTPUTCONFIG ${CMAKE_CONFIGURATION_TYPES})
#     string(TOUPPER ${OUTPUTCONFIG} OUTPUTCONFIG)
#     set(CMAKE_RUNTIME_OUTPUT_DIRECTORY_${OUTPUTCONFIG} ${PROJECT_OUTPUT_DIR}/bin)
#     set(CMAKE_LIBRARY_OUTPUT_DIRECTORY_${OUTPUTCONFIG} ${PROJECT_OUTPUT_DIR}/lib)
#     set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY_${OUTPUTCONFIG} ${PROJECT_OUTPUT_DIR}/lib)
# endforeach()

set(CMAKE_MODULE_PATH "${CMAKE_CURRENT_LIST_DIR}/cmake;${CMAKE_MODULE_PATH}")

# FetchContent added in CMake 3.11, downloads during the configure step
include(FetchContent)
# FetchContent_MakeAvailable was not added until CMake 3.14; use our shim
if(${CMAKE_VERSION} VERSION_LESS 3.14)
  include(cmake/add_FetchContent_MakeAvailable.cmake)
endif()

# 查找并链接库
find_package(nlohmann_json REQUIRED)                                                
find_package(spdlog REQUIRED)                                                       
find_package(Boost REQUIRED COMPONENTS filesystem system asio)                                                        
find_package(PahoMqttCpp REQUIRED)                                                  
find_package(SQLiteCpp REQUIRED)                                                    

find_package(Threads REQUIRED)

# include_directories(    
#     ${PROJECT_SOURCE_DIR}/include
#     ${PROJECT_INCLUDE_DIR}
#     )

add_subdirectory(hydrox)


