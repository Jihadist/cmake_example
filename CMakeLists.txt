cmake_minimum_required(VERSION 3.4...3.18)
project(cmake_example)

add_subdirectory(pybind11)
pybind11_add_module(cmake_example src/main.cpp)

set(EXAMPLE_VERSION_INFO "0.0.1")
target_compile_definitions(cmake_example
    PRIVATE VERSION_INFO=${EXAMPLE_VERSION_INFO})

add_custom_command(TARGET cmake_example PRE_BUILD
    COMMAND ${CMAKE_COMMAND} -E copy
    ${CMAKE_CURRENT_SOURCE_DIR}/tests/test_basic.py
    $<TARGET_FILE_DIR:cmake_example>)

include(CTest)
add_test(
    NAME test_basic
    COMMAND python $<TARGET_FILE_DIR:cmake_example>/test_basic.py
    WORKING_DIRECTORY $<TARGET_FILE_DIR:cmake_example>
)
