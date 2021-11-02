
# CMake build script for REBOUND


cmake_minimum_required(VERSION 3.20)

project(reboundx
    VERSION 3.4.1
    LANGUAGES C
)

list(APPEND CMAKE_MODULE_PATH "${PROJECT_SOURCE_DIR}/cmake")

# Include target definitions.
add_subdirectory(src)

# Install.
include(InstallBasicPackageFiles)
cmakeshift_install_basic_package_files(reboundx
    COMPATIBILITY SameMajorVersion
    DEPENDENCIES
        "rebound"
)
