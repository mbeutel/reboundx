
# CMake build script for REBOUND
# drag force example


cmake_minimum_required(VERSION 3.20)

project(reboundx-examples-modify-orbits
    LANGUAGES C
)

find_package(pthreads REQUIRED)
find_package(rebound REQUIRED)
find_package(reboundx REQUIRED)

add_executable(modify-orbits
    problem.c
)
target_link_libraries(modify-orbits
    PRIVATE
        rebound::rebound
        reboundx::reboundx
)
