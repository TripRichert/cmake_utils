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

get_filename_component(test3_input_file test3/input.txt REALPATH)
read_filelist_use_substitution(test3_output ${test3_input_file})
file(WRITE ${CMAKE_BINARY_DIR}/test3.result "${test3_output}\n")
get_filename_component(test3_expected_file test3/expected.txt.in REALPATH)
configure_file(${test3_expected_file} test3.expected)

get_filename_component(test4_input_file test4/input_deps.txt REALPATH)
add_dependency_tree_file(test4_files ${test4_input_file})
file(WRITE ${CMAKE_BINARY_DIR}/test4.result "${test4_files}\n")
get_filename_component(test4_expected_file test4/expected.txt.in REALPATH)
configure_file(${test4_expected_file} test4.expected)

get_filename_component(test5_input_file test5/input_deps.txt REALPATH)
add_dependency_tree_file(test5_files ${test5_input_file})
get_list_from_dependency_tree(test5_output ${CMAKE_CURRENT_LIST_DIR}/test5/Register_tb.v)
file(WRITE ${CMAKE_BINARY_DIR}/test5.result "${test5_output}\n")
get_filename_component(test5_expected_file test5/expected.txt.in REALPATH)
configure_file(${test5_expected_file} test5.expected)

get_filename_component(compare_tcl compare.tcl REALPATH)
add_test(NAME test1
  COMMAND tclsh ${compare_tcl} ${CMAKE_BINARY_DIR}/test1.expected ${CMAKE_BINARY_DIR}/test1.result
  )

add_test(NAME test2
  COMMAND tclsh ${compare_tcl} ${CMAKE_BINARY_DIR}/test2.expected ${CMAKE_BINARY_DIR}/test2.result
  )


add_test(NAME test3
  COMMAND tclsh ${compare_tcl} ${CMAKE_BINARY_DIR}/test3.expected ${CMAKE_BINARY_DIR}/test3.result
  )


add_test(NAME test4
  COMMAND tclsh ${compare_tcl} ${CMAKE_BINARY_DIR}/test4.expected ${CMAKE_BINARY_DIR}/test4.result
  )

add_test(NAME test5
  COMMAND tclsh ${compare_tcl} ${CMAKE_BINARY_DIR}/test5.expected ${CMAKE_BINARY_DIR}/test5.result
  )
