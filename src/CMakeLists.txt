
# CMake build script for REBOUNDx
# Shared library target


cmake_minimum_required(VERSION 3.20)

# dependencies
find_package(rebound REQUIRED)
find_package(pthreads REQUIRED)

# Get the latest abbreviated commit hash of the working branch
#add_custom_target(GitRevParse
#    COMMAND git rev-parse HEAD
#    WORKING_DIRECTORY ${CMAKE_CURRENT_LIST_DIR}

#add_custom_target(AlwaysCheckGit
#    COMMAND ${CMAKE_COMMAND}
#    -DRUN_CHECK_GIT_VERSION=1
#    -Dpre_configure_dir=${pre_configure_dir}
#    -Dpost_configure_file=${post_configure_dir}
#    -DGIT_HASH_CACHE=${GIT_HASH_CACHE}
#    -P ${CURRENT_LIST_DIR}/CheckGit.cmake
#    BYPRODUCTS ${post_configure_file}
#)
#execute_process(
#    COMMAND git rev-parse HEAD
#    WORKING_DIRECTORY ${CMAKE_CURRENT_LIST_DIR}
#    OUTPUT_VARIABLE GIT_HASH
#    OUTPUT_STRIP_TRAILING_WHITESPACE
#)

# library target
add_library(reboundx SHARED  # `SHARED` implies position-independent code on platforms that need it
    "central_force.c"
    "core.c"
    "exponential_migration.c"
    "gr.c"
    "gravitational_harmonics.c"
    "gr_full.c"
    "gr_potential.c"
    "input.c"
    "integrate_force.c"
    "integrator_euler.c"
    "integrator_implicit_midpoint.c"
    "integrator_rk2.c"
    "integrator_rk4.c"
    "interpolation.c"
    "linkedlist.c"
    "modify_mass.c"
    "modify_orbits_direct.c"
    "modify_orbits_forces.c"
    "output.c"
    "radiation_forces.c"
    "rebxtools.c"
    "steppers.c"
    "tides_constant_time_lag.c"
    "track_min_distance.c"

    # list of shared library exports (required on Windows)
    "reboundx.def"
)

# library properties and dependencies
target_compile_definitions(reboundx
    PUBLIC
        #"REBXGITHASH=${GIT_HASH}"
    PRIVATE
        LIBREBOUNDX
)
target_link_libraries(reboundx
    PUBLIC
        rebound::rebound
    PRIVATE
        $<$<C_COMPILER_ID:GNU>:m>  # link libstdc++ math library ("m") when compiling with GCC
)
target_include_directories(reboundx
    PUBLIC
        "$<BUILD_INTERFACE:${PROJECT_SOURCE_DIR}/src>"
        "$<INSTALL_INTERFACE:include>"
)
target_compile_features(reboundx
    PUBLIC
        c_std_11
)
include(TargetCompileSettings)
cmakeshift_target_compile_settings(reboundx)

# installation: public header files, target
install(
    FILES reboundx.h
    TYPE INCLUDE
)
install(
    TARGETS reboundx
    EXPORT reboundx
    ARCHIVE LIBRARY RUNTIME
)
