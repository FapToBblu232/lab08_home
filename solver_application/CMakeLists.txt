cmake_minimum_required(VERSION 3.22)

project(equation)

include_directories("../formatter_ex_lib/")
include_directories("../solver_lib/")

add_executable(equation equation.cpp)

add_subdirectory("../formatter_ex_lib/" formatter_ex_lib)
add_subdirectory("../solver_lib/" solver_lib)

target_link_libraries(equation formatter_ex_lib)
target_link_libraries(equation solver_lib)