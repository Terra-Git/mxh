# 并将名称保存到 DIR_LIB_SRCS 变量
aux_source_directory(. DIR_LIB_SRCS)

set(PROJECT common)
set(LINK_METHOD SHARED)

# 设置链接库
set(THIRDPART )

# 生成链接库
add_library ( 
  ${PROJECT}
  ${LINK_METHOD}
  ${DIR_LIB_SRCS}
)

target_link_libraries(${PROJECT} ${THIRDPART} )