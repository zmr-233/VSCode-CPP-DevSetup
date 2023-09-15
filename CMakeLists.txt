cmake_minimum_required(VERSION 3.10)
project(IndividualExecutables)

# 添加编译器标志
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -g -O0")
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -g -O0")

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
endforeach()

# 对每个文件夹内的文件进行处理
foreach(SOURCE ${DIR_SOURCES})
    get_filename_component(DIR_NAME ${SOURCE} PATH)
    get_filename_component(PARENT_DIR_NAME ${DIR_NAME} NAME)
    get_filename_component(EXECUTABLE_NAME ${SOURCE} NAME_WE)
    file(GLOB LOCAL_SOURCES "${DIR_NAME}/*.cpp" "${DIR_NAME}/*.c")
    add_executable(${EXECUTABLE_NAME}_out ${LOCAL_SOURCES})
    set_target_properties(${EXECUTABLE_NAME}_out PROPERTIES RUNTIME_OUTPUT_DIRECTORY "${CMAKE_SOURCE_DIR}/${PARENT_DIR_NAME}/bin")

endforeach()

# 在每次CMake配置时都删除CompilerIdC和CompilerIdCXX目录
execute_process(
    COMMAND ${CMAKE_COMMAND} -E remove_directory "${CMAKE_SOURCE_DIR}/CompilerIdC"
    COMMAND ${CMAKE_COMMAND} -E remove_directory "${CMAKE_SOURCE_DIR}/CompilerIdCXX"
)
