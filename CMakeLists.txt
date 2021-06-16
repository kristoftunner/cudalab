cmake_minimum_required(VERSION 3.10)

project(mbr3d_main CXX CUDA)

set(CMAKE_CXX_STANDARD 17)
# set(CMAKE_VERBOSE_MAKEFILE ON)

# add_subdirectory(ext)

set(THREADS_PREFER_PTHREAD_FLAG OFF)

if(NOT CMAKE_BUILD_TYPE)
  set(CMAKE_BUILD_TYPE Release)
endif()

set(CMAKE_CXX_FLAGS "-Wall -Wextra")
set(CMAKE_CXX_FLAGS_DEBUG "-g")
set(CMAKE_CXX_FLAGS_RELEASE "-O3")

set(SRC_FILES
    src/time_measure.cpp
    src/main.cpp
    src/cuda_main.cu
    src/memalloc_host.cu 
)

SET(CUDA_SEPARABLE_COMPILATION ON)
########################### ADD LIB ################################
add_library(${PROJECT_NAME} ${SRC_FILES})

set_property(TARGET ${PROJECT_NAME} PROPERTY CXX_STANDARD 17)
set_property(TARGET ${PROJECT_NAME} PROPERTY CUDA_STANDARD 14)
target_include_directories(${PROJECT_NAME} PUBLIC include ${CMAKE_CURRENT_BINARY_DIR})
# set_target_properties(${PROJECT_NAME}  PROPERTIES POSITION_INDEPENDENT_CODE TRUE)
target_compile_options(${PROJECT_NAME} PRIVATE 
    $<$<COMPILE_LANGUAGE:CUDA>:--default-stream per-thread>
)

########################### ADD EXE ################################
add_executable(${PROJECT_NAME}_runner src/main.cpp)
target_link_libraries(${PROJECT_NAME}_runner
    ${PROJECT_NAME}
    config++
    boost_program_options
)

########################### ADD TEST ################################
# set(TEST_FILES
#     test/boost_multi_array_test.cu
#     src/cuda_kernels.cu
#     src/abs_dfe.cpp
#     test/abs_dfe_test.cpp
#     test/json2fbs.cpp
# )
# add_executable(${PROJECT_NAME}_testrunner ${TEST_FILES})
# target_include_directories(${PROJECT_NAME}_testrunner PUBLIC include)
# target_link_libraries(${PROJECT_NAME}_testrunner
#     utils 
#     pipeline
#     gtest 
#     gtest_main
#     protocol_mbr_int
#     flatbuffers
# )
# target_compile_options(${PROJECT_NAME}_testrunner PRIVATE 
#     $<$<COMPILE_LANGUAGE:CUDA>:--default-stream legacy>
# )

# add_executable(${PROJECT_NAME}_dsp_runner src/mbr3d_main_dsp_runner.cpp)
# target_link_libraries(${PROJECT_NAME}_dsp_runner 
#     ${PROJECT_NAME}
#     boost_program_options
#     config++
# )

########################### ADD BENCHMARK ################################
# set(BM_FILES
#     test/boost_multi_array_benchmark.cpp
# )
# add_executable(${PROJECT_NAME}_benchmark ${BM_FILES})
# target_link_libraries(${PROJECT_NAME}_benchmark
#     utils
#     benchmark::benchmark
# )
# target_include_directories(${PROJECT_NAME}_benchmark PRIVATE
#     benchmark::benchmark
# )


# add_executable(ptree_trial test/ptree_libconfig_trial.cpp)
# target_link_libraries(ptree_trial PRIVATE 
#     config++
#     spdtrace    
#     utils
# )
