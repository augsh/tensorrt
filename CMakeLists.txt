cmake_minimum_required(VERSION 3.20)

project(tensorrt)

# cuda
find_package(CUDA REQUIRED)
message("CUDA_LIBRARIES:${CUDA_LIBRARIES}")
message("CUDA_INCLUDE_DIRS:${CUDA_INCLUDE_DIRS}")
include_directories(${CUDA_INCLUDE_DIRS})
if(CUDA_FOUND)
  message(STATUS "Found CUDA libraries")
else()
  message(FATAL_ERROR "Not found CUDA libraries")
endif()

add_library(${PROJECT_NAME} INTERFACE IMPORTED GLOBAL)
target_include_directories(
  ${PROJECT_NAME} INTERFACE ${CMAKE_CURRENT_SOURCE_DIR}/include
                            ${CUDA_INCLUDE_DIRS})
file(GLOB_RECURSE SO_FILES "lib/*.so")
target_link_libraries(${PROJECT_NAME} INTERFACE ${SO_FILES} ${CUDA_LIBRARIES})

if(NOT ${OUT} STREQUAL "")
  message("Set output directory to ${OUT}")
  file(GLOB_RECURSE BINS "bin/*")
  file(COPY ${BINS} DESTINATION ${OUT_BIN}/tensorrt)
  file(GLOB_RECURSE LIBS "lib/lib*")
  file(COPY ${LIBS} DESTINATION ${OUT_LIB})
endif()
