project(tensorboard_logger)

find_package(Protobuf REQUIRED)

file(GLOB protos "proto/*.proto")

# check if protobuf was found
if(NOT PROTOBUF_FOUND)
    message (FATAL_ERROR "Cannot find Protobuf")
endif()

file(GLOB sources "src/*.cc")

# Generate the .h and .cxx files
PROTOBUF_GENERATE_CPP(PROTO_SRCS PROTO_HDRS ${protos})

set(TENSORBOARD_INCLUDE_PATH 
  ${PROJECT_SOURCE_DIR}/include
  ${PROJECT_BINARY_DIR})

include_directories(${TENSORBOARD_INCLUDE_PATH})

add_library(tensorboard SHARED 
  ${sources}
  ${PROTO_SRCS})
target_link_libraries(tensorboard -lprotobuf)