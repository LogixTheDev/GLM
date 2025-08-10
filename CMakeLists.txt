cmake_minimum_required(VERSION 3.18)
project(glm VERSION 0.0.0 LANGUAGES CXX)  # or parse version if you want

# Header-only target
add_library(glm INTERFACE)
add_library(glm::glm ALIAS glm)

target_include_directories(glm
        INTERFACE
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
        $<INSTALL_INTERFACE:include>)

# Optional toggles (uncomment if you want defaults baked in)
# target_compile_definitions(glm INTERFACE GLM_FORCE_RADIANS=1 GLM_ENABLE_EXPERIMENTAL=1)

# -------- Install / package (so find_package works) --------
include(CMakePackageConfigHelpers)

install(TARGETS glm
        EXPORT glmTargets
        INCLUDES DESTINATION include)

install(DIRECTORY include/ DESTINATION include)

write_basic_package_version_file(
        "${CMAKE_CURRENT_BINARY_DIR}/glmConfigVersion.cmake"
        VERSION ${PROJECT_VERSION}
        COMPATIBILITY SameMajorVersion)

file(WRITE "${CMAKE_CURRENT_BINARY_DIR}/glmConfig.cmake"
        "@PACKAGE_INIT@\ninclude(\"\${CMAKE_CURRENT_LIST_DIR}/glmTargets.cmake\")\n")

install(EXPORT glmTargets
        NAMESPACE glm::
        FILE glmTargets.cmake
        DESTINATION lib/cmake/glm)

install(FILES
        "${CMAKE_CURRENT_BINARY_DIR}/glmConfig.cmake"
        "${CMAKE_CURRENT_BINARY_DIR}/glmConfigVersion.cmake"
        DESTINATION lib/cmake/glm)

# Optional: register for local find_package() without paths
export(PACKAGE glm)
