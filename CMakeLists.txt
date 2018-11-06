cmake_minimum_required(VERSION 3.5)
project(OpenCV_Test)

set(CMAKE_CXX_STANDARD 11)

set(OpenCV_DIR "/usr/share/OpenCV")
find_package(OpenCV REQUIRED)
include_directories(${OpenCV_INCLUDE_DIR})
link_directories(${OpenCV_LIBRARY_DIRS})
add_definitions(${OpenCV_DEFINITIONS})

set(SOURCE_FILES main.cpp)
add_executable(OpenCV_Test ${SOURCE_FILES})
target_link_libraries(OpenCV_Test ${OpenCV_LIBS})
