cmake_minimum_required(VERSION 3.24)
set(CMAKE_CUDA_ARCHITECTURES 75)
project(retinaface CUDA)
option(CUDA_USE_STATIC_CUDA_RUNTIME OFF)
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_BUILD_TYPE Debug)

include_directories(${PROJECT_SOURCE_DIR}/include)

find_package(OpenCV REQUIRED)
include_directories(${OpenCV_INCLUDE_DIRS})

include_directories(C:\\Program\ Files\\NVIDIA\\TensorRT-8.5.3.1\\include)
link_directories(C:\\Program\ Files\\NVIDIA\\TensorRT-8.5.3.1\\lib)


find_package(CUDA REQUIRED)
cuda_add_library(decodeplugin SHARED ${PROJECT_SOURCE_DIR}/decode.cu)
target_link_libraries(decodeplugin nvinfer cudart)



add_executable(retina_r50 ${PROJECT_SOURCE_DIR}/calibrator.cpp ${PROJECT_SOURCE_DIR}/retina_r50.cpp)
target_link_libraries(retina_r50 nvinfer)
target_link_libraries(retina_r50 cudart)
target_link_libraries(retina_r50 decodeplugin)
target_link_libraries(retina_r50 ${OpenCV_LIBRARIES})
set_target_properties(retina_r50 PROPERTIES
        CUDA_SEPARABLE_COMPILATION ON)

add_executable(retina_mnet ${PROJECT_SOURCE_DIR}/calibrator.cpp ${PROJECT_SOURCE_DIR}/retina_mnet.cpp)
target_link_libraries(retina_mnet nvinfer)
target_link_libraries(retina_mnet cudart)
target_link_libraries(retina_mnet decodeplugin)
target_link_libraries(retina_mnet ${OpenCV_LIBRARIES})
#set_target_properties(retina_mnet PROPERTIES
#        CUDA_SEPARABLE_COMPILATION ON)
add_definitions(-O2)


#cmake_minimum_required(VERSION 2.6)
#
#project(retinaface)
#
#add_definitions(-std=c++11)
#
#option(CUDA_USE_STATIC_CUDA_RUNTIME OFF)
#set(CMAKE_CXX_STANDARD 11)
#set(CMAKE_BUILD_TYPE Debug)
#
#find_package(CUDA REQUIRED)
#
#if (CMAKE_SYSTEM_PROCESSOR MATCHES "aarch64")
#    message("embed_platform on")
#    include_directories(/usr/local/cuda/targets/aarch64-linux/include)
#    link_directories(/usr/local/cuda/targets/aarch64-linux/lib)
#else()
#    message("embed_platform off")
#    include_directories(C:\\Program\ Files\\NVIDIA\ GPU\ Computing\ Toolkit\\CUDA\\v11.8\\include)
#    include_directories(C:\\Program\ Files\\NVIDIA\\TensorRT-8.5.3.1\\include)
#
#    link_directories(C:\\Program\ Files\\NVIDIA\ GPU\ Computing\ Toolkit\\CUDA\\v11.8\\lib\\x64)
#    link_directories(C:\\Program\ Files\\NVIDIA\\TensorRT-8.5.3.1\\lib)
#endif()
#
#set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11 -Wall -D_MWAITXINTRIN_H_INCLUDED")
##set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11 ")
#cuda_add_library(decodeplugin SHARED ${PROJECT_SOURCE_DIR}/decode.cu)
#target_link_libraries(decodeplugin nvinfer cudart)
#
#find_package(OpenCV)
#include_directories(${OpenCV_INCLUDE_DIRS})
#
#add_executable(retina_r50 ${PROJECT_SOURCE_DIR}/calibrator.cpp ${PROJECT_SOURCE_DIR}/retina_r50.cpp)
#target_link_libraries(retina_r50 nvinfer)
#target_link_libraries(retina_r50 cudart)
#target_link_libraries(retina_r50 decodeplugin)
#target_link_libraries(retina_r50 ${OpenCV_LIBRARIES})
#
#add_executable(retina_mnet ${PROJECT_SOURCE_DIR}/calibrator.cpp ${PROJECT_SOURCE_DIR}/retina_mnet.cpp)
#target_link_libraries(retina_mnet nvinfer)
#target_link_libraries(retina_mnet cudart)
#target_link_libraries(retina_mnet decodeplugin)
#target_link_libraries(retina_mnet ${OpenCV_LIBRARIES})
#
#add_definitions(-O2 -pthread)

