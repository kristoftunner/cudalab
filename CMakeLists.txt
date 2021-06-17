cmake_minimum_required(VERSION 3.10)

project(cudalab CXX CUDA)

add_executable(${PROJECT_NAME} src/main.cpp)

#add_subdirectory(ext/devIL/DevIL)

set(CMAKE_CXX_STANDARD 17)

set(CMAKE_CXX_FLAGS "-Wall -Wextra")
set(CMAKE_CXX_FLAGS_DEBUG "-g")
set(CMAKE_CXX_FLAGS_RELEASE "-O3")

SET(CUDA_SEPARABLE_COMPILATION ON)
set_property(TARGET ${PROJECT_NAME} PROPERTY CXX_STANDARD 17)
set_property(TARGET ${PROJECT_NAME} PROPERTY CUDA_STANDARD 14)

find_package(DevIL REQUIRED)

set(SRC_FILES
    src/cuda_main.cu
    src/memalloc_host.cu
    src/time_measure.cpp
)

add_library(${PROJECT_NAME}_lib ${SRC_FILES})

target_include_directories(${PROJECT_NAME} 
    PRIVATE ${IL_INCLUDE_DIR}
)

#target_link_directories(${PROJECT_NAME}
#  PRIVATE ext/devIL/DevIL
#)
target_link_libraries(${PROJECT_NAME} 
    PRIVATE ${IL_LIBRARIES}
    PRIVATE ${ILU_LIBRARIES}
    PRIVATE ${ILUT_LIBRARIES}
)
