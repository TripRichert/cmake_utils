#vivado's version of cmake is 3.3
cmake_minimum_required(VERSION 3.3)
project(project)
#do not allow in source build
set(CMAKE_DISABLE_SOURCE_CHANGES ON)
set(CMAKE_DISABLE_IN_SOURCE_BUILD ON)
include(CTest)
enable_testing()


include(../util_functions.cmake)

get_filename_component(test1_input_file test1/input.txt REALPATH)
read_filelist(test1_output ${test1_input_file})
file(WRITE ${CMAKE_BINARY_DIR}/test1.result "${test1_output}\n")
get_filename_component(test1_expected_file test1/expected.txt.in REALPATH)
configure_file(${test1_expected_file} test1.expected)

get_filename_component(test2_input_file test2/input.txt REALPATH)
read_filelist(test2_output ${test2_input_file})
file(WRITE ${CMAKE_BINARY_DIR}/test2.result "${test2_output}\n")
get_filename_component(test2_expected_file test2/expected.txt.in REALPATH)
configure_file(${test2_expected_file} test2.expected)


get_filename_component(compare_tcl compare.tcl REALPATH)
add_test(NAME test1
  COMMAND tclsh ${compare_tcl} ${CMAKE_BINARY_DIR}/test1.expected ${CMAKE_BINARY_DIR}/test1.result
  )

add_test(NAME test2
  COMMAND tclsh ${compare_tcl} ${CMAKE_BINARY_DIR}/test2.expected ${CMAKE_BINARY_DIR}/test2.result
  )
