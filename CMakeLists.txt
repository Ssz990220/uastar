cmake_minimum_required(VERSION 3.27)
set(CMAKE_TOOLCHAIN_FILE "C:/vcpkg/scripts/buildsystems/vcpkg.cmake")

if(NOT DEFINED CMAKE_CUDA_ARCHITECTURES)
  set(CMAKE_CUDA_ARCHITECTURES 80)
endif()

project(uastar LANGUAGES CUDA CXX)

find_package(Boost COMPONENTS program_options system filesystem REQUIRED)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED On)
set(CMAKE_CXX_EXTENSIONS Off)
# set(CMAKE_CUDA_FLAGS_DEBUG ${CMAKE_CUDA_FLAGS_DEBUG} -lineinfo)
# set(CMAKE_CXX_FLAGS_DEBUG "${CMAKE_CXX_FLAGS_DEBUG} -rdynamic -Wall -DDEBUG -fsanitize=address")
# set(CMAKE_CUDA_FLAGS_RELEASE ${CMAKE_CUDA_FLAGS_RELEASE} -O2)
# set(CMAKE_CXX_FLAGS_RELEASE "${CMAKE_CXX_FLAGS_RELEASE} -O2")

include_directories(libs/moderngpu/include)
include_directories(libs/bitmap)
include_directories(src)
include_directories(${Boost_INCLUDE_DIRS})
link_directories(${Boost_LIBRARY_DIRS} "C:/Program Files/Microsoft Visual Studio/2022/Community/VC/Tools/MSVC/14.37.32822/include"
"C:/Program Files (x86)/Windows Kits/10/Include/10.0.22621.0/ucrt")

set(
    moderngpu_source
    libs/moderngpu/src/mgpucontext.cu
    libs/moderngpu/src/mgpuutil.cpp
)

add_executable(
    uastar
    src/main.cpp
    src/pathway/pathway.cpp
    src/pathway/pathway.hpp
    src/pathway/CPU-solver.cpp
    src/pathway/CPU-solver.hpp
    src/pathway/GPU-solver.cu
    src/pathway/GPU-solver.hpp
    src/pathway/input/custom.cpp
    src/pathway/input/custom.hpp
    src/pathway/input/zigzag.cpp
    src/pathway/input/zigzag.hpp
    src/puzzle/puzzle.cu
    src/puzzle/puzzle.cuh
    src/puzzle/database.cpp
    src/puzzle/database.hpp
    ${moderngpu_source}
)

set_target_properties(uastar PROPERTIES CUDA_SEPARABLE_COMPILATION ON)

set_property(TARGET uastar PROPERTY CUDA_STANDARD 17)

# add_executable(
#     demo-sort
#     utils/modern-sort.cu
#     ${moderngpu_source}
# )

target_link_libraries(
    uastar
    ${Boost_LIBRARIES}
)
