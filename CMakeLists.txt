project(tensorboard_logger)

file(GLOB PROTOBUF_DEFINITION_FILES "*.proto")

execute_process(
  COMMAND "protoc -Iproto ${PROTOBUF_DEFINITION_FILES} --cpp_out=proto"
)

file(GLOB sources "proto/*.proto" "src/*.pb.cc")

set(TENSORBOARD_INCLUDE_PATH ${PROJECT_SOURCE_DIR}/include)

include_directories(${TENSORBOARD_INCLUDE_PATH})

add_library(tensorboard SHARED 
  ${PROTOBUF_DEFINITION_FILES}
  ${sources}
  src/tensorboard_logger.cc 
  src/crc.cc)
target_link_libraries(tensorboard -lprotobuf)