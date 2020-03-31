cmake_minimum_required(VERSION 3.8)
project(GPUBURN LANGUAGES CUDA CXX)
find_package(PkgConfig)
string(REGEX MATCH "^[0-9]+\.[0-9]+" CUDA_VERSION ${CMAKE_CUDA_COMPILER_VERSION})
pkg_check_modules(CUBLAS REQUIRED IMPORTED_TARGET cublas-${CUDA_VERSION})
pkg_check_modules(CUDART REQUIRED IMPORTED_TARGET cudart-${CUDA_VERSION})
add_library(compare OBJECT compare.cu)
set_property(TARGET compare PROPERTY CUDA_PTX_COMPILATION ON)
add_executable(gpu_burn gpu_burn-drv.cpp)
target_link_libraries(gpu_burn PkgConfig::CUBLAS PkgConfig::CUDART -lcuda)

if(NOT DEFINED GPUBURN_INSTALLDIR)
  set(GPUBURN_INSTALLDIR "gpu_burn")
endif()

install(TARGETS gpu_burn RUNTIME DESTINATION "${GPUBURN_INSTALLDIR}")
# Would like to use install(TARGETS compare...) here but that preserves
# the subdirectory structure, which we don't want.
install(FILES $<TARGET_OBJECTS:compare> DESTINATION "${GPUBURN_INSTALLDIR}")
