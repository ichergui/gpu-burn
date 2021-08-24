cmake_minimum_required(VERSION 3.8)
project(GPU_BURN LANGUAGES CXX CUDA)

# otherwise the installation directories are empty by default
include(GNUInstallDirs)

find_package(CUDA REQUIRED)

set(GPU_BURN_SOURCES
    gpu_burn-drv.cpp
    compare.cu
)

add_executable(gpu_burn ${GPU_BURN_SOURCES})
set_target_properties(gpu_burn PROPERTIES CUDA_SEPARABLE_COMPILATION ON)
target_include_directories(gpu_burn PUBLIC ${CUDA_INCLUDE_DIRS})
target_link_libraries(gpu_burn PUBLIC cuda ${CUDA_CUDART_LIBRARY} ${CUDA_CUBLAS_LIBRARIES})

install (TARGETS gpu_burn DESTINATION ${CMAKE_INSTALL_BINDIR})
