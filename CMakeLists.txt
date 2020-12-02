cmake_minimum_required(VERSION 3.2 FATAL_ERROR)
cmake_policy(VERSION 3.2)


file(READ "glm/detail/setup.hpp" GLM_SETUP_FILE)
string(REGEX MATCH "#define[ ]+GLM_VERSION_MAJOR[ ]+([0-9]+)" _ ${GLM_SETUP_FILE})
set(GLM_VERSION_MAJOR "${CMAKE_MATCH_1}")
string(REGEX MATCH "#define[ ]+GLM_VERSION_MINOR[ ]+([0-9]+)" _ ${GLM_SETUP_FILE})
set(GLM_VERSION_MINOR "${CMAKE_MATCH_1}")
string(REGEX MATCH "#define[ ]+GLM_VERSION_PATCH[ ]+([0-9]+)" _ ${GLM_SETUP_FILE})
set(GLM_VERSION_PATCH "${CMAKE_MATCH_1}")
string(REGEX MATCH "#define[ ]+GLM_VERSION_REVISION[ ]+([0-9]+)" _ ${GLM_SETUP_FILE})
set(GLM_VERSION_REVISION "${CMAKE_MATCH_1}")

set(GLM_VERSION ${GLM_VERSION_MAJOR}.${GLM_VERSION_MINOR}.${GLM_VERSION_PATCH}.${GLM_VERSION_REVISION})
project(glm VERSION ${GLM_VERSION} LANGUAGES CXX)
message(STATUS "GLM: Version " ${GLM_VERSION})

enable_testing()

add_subdirectory(glm)
add_library(glm::glm ALIAS glm)

if(${CMAKE_SOURCE_DIR} STREQUAL ${CMAKE_CURRENT_SOURCE_DIR})

add_subdirectory(test)

endif(${CMAKE_SOURCE_DIR} STREQUAL ${CMAKE_CURRENT_SOURCE_DIR})

#########################################################################################
# CMake Package Configuration
#########################################################################################
# Helpers functions for creating config files that can be included by other projects
include(CMakePackageConfigHelpers)

# Create config package
configure_package_config_file(
    "cmake/Config.cmake.in"
    "${CMAKE_CURRENT_BINARY_DIR}/glmConfig.cmake"
    INSTALL_DESTINATION "lib/cmake/glm")
write_basic_package_version_file(
    "${CMAKE_CURRENT_BINARY_DIR}/glmConfigVersion.cmake"
    COMPATIBILITY SameMajorVersion)

# Install config package
install(FILES "${CMAKE_CURRENT_BINARY_DIR}/glmConfig.cmake"
              "${CMAKE_CURRENT_BINARY_DIR}/glmConfigVersion.cmake"
        DESTINATION "lib/cmake/glm")
install(EXPORT "glmTargets"
        NAMESPACE "glm::"
        DESTINATION "lib/cmake/glm")
