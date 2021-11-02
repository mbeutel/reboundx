
# CMake build script for REBOUND
# drag force example


cmake_minimum_required(VERSION 3.20)

project(reboundx-examples-parameter_interpolation
    LANGUAGES C
)

find_package(pthreads REQUIRED)
find_package(rebound REQUIRED)
find_package(reboundx REQUIRED)

add_executable(parameter_interpolation
    problem.c
)
target_link_libraries(parameter_interpolation
    PRIVATE
        rebound::rebound
        reboundx::reboundx
)
