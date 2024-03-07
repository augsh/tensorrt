project(tensorrt)

# cuda
find_package(CUDA REQUIRED)
if(CUDA_FOUND)
  include_directories(${CUDA_INCLUDE_DIRS})

  # tensorrt
  set(TENSORRT_INCLUDE_DIRS "${CMAKE_CURRENT_SOURCE_DIR}/include")
  file(GLOB TENSORRT_LIBS "${CMAKE_CURRENT_SOURCE_DIR}/lib/lib*.so")
  # message("TensorRT libraries: ${TENSORRT_LIBS}")

  add_library(${PROJECT_NAME} INTERFACE IMPORTED GLOBAL)
  target_link_libraries(${PROJECT_NAME} INTERFACE ${TENSORRT_LIBS} ${CUDA_LIBRARIES})
  target_include_directories(
    ${PROJECT_NAME} 
    INTERFACE ${TENSORRT_INCLUDE_DIRS}
    INTERFACE ${CUDA_INCLUDE_DIRS}
  )

  add_files(${OUT_3RDLIB} "lib/lib*")
  add_files(${OUT_3RDLIB}/stubs "lib/stubs/lib*")
endif()
