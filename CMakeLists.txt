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

# tensorrt
set(TENSORRT_INCLUDE_DIRS "${CMAKE_CURRENT_SOURCE_DIR}/include")
file(GLOB TENSORRT_LIBS "${CMAKE_CURRENT_SOURCE_DIR}/lib/lib*.so")
# message("TensorRT libraries: ${TENSORRT_LIBS}")

add_library(${PROJECT_NAME} INTERFACE IMPORTED GLOBAL)
target_include_directories(${PROJECT_NAME} INTERFACE ${TENSORRT_INCLUDE_DIRS}
                                                     ${CUDA_INCLUDE_DIRS})
target_link_libraries(${PROJECT_NAME} INTERFACE ${TENSORRT_LIBS}
                                                ${CUDA_LIBRARIES})

add_files(${OUT_3RDLIB} "lib/lib*")
add_files(${OUT_3RDLIB}/stubs "lib/stubs/lib*")
