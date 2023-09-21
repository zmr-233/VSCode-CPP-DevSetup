cmake_minimum_required(VERSION 3.10)
project(IndividualExecutables)

# 添加编译器标志
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -g -O0")
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -g -O0")

# 设置一个选项来决定是否使用Connector/C++或mysql++
option(USE_CONNECTOR_CPP "Use Connector/C++ for MySQL support" OFF) # 默认为OFF
option(USE_MYSQLPP "Use mysql++ for MySQL support" OFF) # 默认为OFF

# 根据上面的选项生成config.h文件
configure_file(${CMAKE_SOURCE_DIR}/config.h.in ${CMAKE_BINARY_DIR}/config.h)

# 将二进制目录添加到搜索路径，以便我们可以找到config.h
include_directories(${CMAKE_BINARY_DIR})

# 为Connector/C++添加头文件目录
if(USE_CONNECTOR_CPP)
    include_directories(/usr/include/cppconn)
endif()
# 为mysql++添加头文件目录
if(USE_MYSQLPP)
    include_directories(/usr/include/mysql++)
    include_directories(/usr/include/mysql)
endif()

# 收集所有 .cpp 和 .c 文件
file(GLOB SOURCES "*.cpp" "*.c")
file(GLOB_RECURSE DIR_SOURCES "*/**.cpp" "*/**.c")

# 为每个根目录下的源文件创建一个可执行文件
foreach(SOURCE ${SOURCES})
    # 获取文件名（不带扩展名）
    get_filename_component(EXECUTABLE_NAME ${SOURCE} NAME_WE)
    # 添加可执行文件目标
    add_executable(${EXECUTABLE_NAME}_out ${SOURCE})
    set_target_properties(${EXECUTABLE_NAME}_out PROPERTIES RUNTIME_OUTPUT_DIRECTORY "${CMAKE_SOURCE_DIR}/bin")

    if(USE_CONNECTOR_CPP)
    target_link_libraries(${EXECUTABLE_NAME}_out mysqlcppconn)
endif()

if(USE_MYSQLPP)
    target_link_libraries(${EXECUTABLE_NAME}_out mysqlpp)
endif()

endforeach()

# 对每个文件夹内的文件进行处理
foreach(SOURCE ${DIR_SOURCES})
    get_filename_component(DIR_NAME ${SOURCE} PATH)
    get_filename_component(PARENT_DIR_NAME ${DIR_NAME} NAME)
    get_filename_component(EXECUTABLE_NAME ${SOURCE} NAME_WE)
    file(GLOB LOCAL_SOURCES "${DIR_NAME}/*.cpp" "${DIR_NAME}/*.c")
    add_executable(${EXECUTABLE_NAME}_out ${LOCAL_SOURCES})
    set_target_properties(${EXECUTABLE_NAME}_out PROPERTIES RUNTIME_OUTPUT_DIRECTORY "${CMAKE_SOURCE_DIR}/${PARENT_DIR_NAME}/bin")

    if(USE_CONNECTOR_CPP)
    target_link_libraries(${EXECUTABLE_NAME}_out mysqlcppconn)
endif()

if(USE_MYSQLPP)
    target_link_libraries(${EXECUTABLE_NAME}_out mysqlpp)
endif()

endforeach()

# 在每次CMake配置时都删除CompilerIdC和CompilerIdCXX目录
execute_process(
    COMMAND ${CMAKE_COMMAND} -E remove_directory "${CMAKE_SOURCE_DIR}/CompilerIdC"
    COMMAND ${CMAKE_COMMAND} -E remove_directory "${CMAKE_SOURCE_DIR}/CompilerIdCXX"
)

# 为 'cleanall' 目标添加一个前置步骤，用于删除所有 'bin' 文件夹
add_custom_target(cleanall_pre
    COMMAND ${CMAKE_COMMAND} -E remove_directory "${CMAKE_SOURCE_DIR}/bin"
    COMMENT "正在删除 bin 目录..."
)

# 对于每个包含源文件的目录，将其删除命令添加到 'cleanall_pre' 目标中
foreach(SOURCE ${DIR_SOURCES})
    get_filename_component(DIR_NAME ${SOURCE} PATH)
    get_filename_component(PARENT_DIR_NAME ${DIR_NAME} NAME)
    add_custom_command(
        TARGET cleanall_pre
        COMMAND ${CMAKE_COMMAND} -E remove_directory "${CMAKE_SOURCE_DIR}/${PARENT_DIR_NAME}/bin"
    )
endforeach()

# 为 'cleanall' 目标添加一个命令，使其首先执行 'cleanall_pre'，然后执行默认的 'clean' 操作
add_custom_target(cleanall
    COMMAND ${CMAKE_BUILD_TOOL} clean
    DEPENDS cleanall_pre
)
