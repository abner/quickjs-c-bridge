cmake_minimum_required(VERSION 3.10)

include("${CMAKE_CURRENT_SOURCE_DIR}/../cxx/quickjs.cmake")
#target_compile_options(quickjs PRIVATE "-fPIC")

set(PROJECT_NAME "quickjs_c_bridge")
project(${PROJECT_NAME} LANGUAGES CXX C)

set(PLUGIN_NAME "${PROJECT_NAME}_plugin")

set(CMAKE_C_COMPILER "gcc")
set(CMAKE_CXX_COMPILER "g++")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++17 -DANDROID_STL=c++_static -DDEBUGGER_SERVER_PORT=${DEBUGGER_SERVER_PORT}")
#set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -Wno-error=incompatible-pointer-types-discards-qualifiers")
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -Wno-int-conversion")
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -Wno-unused-variable")
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -Wno-unused-but-set-variable")
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -Wno-maybe-uninitialized")

add_library(${PLUGIN_NAME} SHARED
  "${CXX_LIB_DIR}/libfastdev_quickjs_runtime.cpp"
)

target_compile_features(${PLUGIN_NAME} PUBLIC cxx_std_17)
set_target_properties(${PLUGIN_NAME} PROPERTIES
  CXX_VISIBILITY_PRESET hidden)
target_compile_definitions(${PLUGIN_NAME} PRIVATE FLUTTER_PLUGIN_IMPL)
target_include_directories(${PLUGIN_NAME} INTERFACE
  "${CMAKE_CURRENT_SOURCE_DIR}/include")

#target_link_libraries(${PLUGIN_NAME} PRIVATE quickjs)
