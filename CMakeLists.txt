cmake_minimum_required(VERSION 3.14)

set(PACKAGE_BINARY_INSTALL_DIR "bin")
set(PACKAGE_DOCS_INSTALL_DIR "usr/share/libnoise/doc")
set(PACKAGE_INCLUDE_INSTALL_DIR "include")
set(PACKAGE_LIBRARY_INSTALL_DIR "lib")
set(PACKAGE_CONFIG_INSTALL_DIR "${PACKAGE_LIBRARY_INSTALL_DIR}/Noise/cmake")

option(BUILD_DOC ON "Build and install Doxygen documentation")
option(ENABLE_TESTING OFF "Build unit tests. Requires the googletest library.")

project(libnoise
        LANGUAGES CXX
        VERSION 1.0.0
        )

include(GenerateExportHeader)

# Major/Minor version number of the SO interface
set(NOISE_SO_VERSION "0.3")

set(CMAKE_POSITION_INDEPENDENT_CODE ON)

add_subdirectory(src)
add_subdirectory(doc)


### Build targets

# Shared library (.so, .dylib or .dll)
add_library(libnoise_shared SHARED
        $<TARGET_OBJECTS:libnoise_src>
        $<TARGET_OBJECTS:libnoise_model>
        $<TARGET_OBJECTS:libnoise_module>
        $<$<PLATFORM_ID:Windows>:$<TARGET_OBJECTS:libnoise_win32_dll>>
        )

generate_export_header(libnoise_shared
        BASE_NAME LIBNOISE
        )

set_target_properties(libnoise_shared PROPERTIES
        OUTPUT_NAME "noise"
        SOVERSION "${NOISE_SO_VERSION}"
        INTERFACE_libnoise_shared_MAJOR_VERSION 0
        INTERFACE_libnoise_shared_MINOR_VERSION 3
        COMPATIBLE_INTERFACE_STRING "${NOISE_SO_VERSION}"
        EXPORT_NAME shared
        )


# Static library (.a or .lib)
add_library(libnoise_static STATIC
        $<TARGET_OBJECTS:libnoise_src>
        $<TARGET_OBJECTS:libnoise_model>
        $<TARGET_OBJECTS:libnoise_module>
        )

set_target_properties(libnoise_static PROPERTIES
        OUTPUT_NAME "noise"
        INTERFACE_libnoise_static_MAJOR_VERSION 0
        INTERFACE_libnoise_static_MINOR_VERSION 3
        COMPATIBLE_INTERFACE_STRING "${NOISE_SO_VERSION}"
        EXPORT_NAME static
        )


### Installation

# Library files (.a/.so/.dylib/.dll/.lib)
install(TARGETS libnoise_shared libnoise_static
        EXPORT NoiseTargets
        LIBRARY DESTINATION "${PACKAGE_LIBRARY_INSTALL_DIR}"
        ARCHIVE DESTINATION "${PACKAGE_LIBRARY_INSTALL_DIR}"
        RUNTIME DESTINATION "${PACKAGE_BINARY_INSTALL_DIR}"
        INCLUDES DESTINATION "${PACKAGE_INCLUDE_INSTALL_DIR}"
        COMPONENT Binaries
        )

# Header structure
install(DIRECTORY src/
        DESTINATION "${PACKAGE_INCLUDE_INSTALL_DIR}/noise"
        COMPONENT Develop
        FILES_MATCHING
            PATTERN "*.h"
            PATTERN "*/win32" EXCLUDE
        )


# Create CMake package files for easy import using find_package()
include(CMakePackageConfigHelpers)

configure_package_config_file(cmake/NoiseConfig.cmake.in
        "${CMAKE_CURRENT_BINARY_DIR}/Noise/NoiseConfig.cmake"
        INSTALL_DESTINATION "${PACKAGE_CONFIG_INSTALL_DIR}"
        PATH_VARS PACKAGE_INCLUDE_INSTALL_DIR
)

write_basic_package_version_file(
        "${CMAKE_CURRENT_BINARY_DIR}/Noise/NoiseConfigVersion.cmake"
        VERSION ${PROJECT_VERSION}
        COMPATIBILITY SameMajorVersion
)

install(FILES
        "${CMAKE_CURRENT_BINARY_DIR}/Noise/NoiseConfig.cmake"
        "${CMAKE_CURRENT_BINARY_DIR}/Noise/NoiseConfigVersion.cmake"
        DESTINATION "${PACKAGE_CONFIG_INSTALL_DIR}"
        )

export(EXPORT NoiseTargets
        FILE "${CMAKE_CURRENT_BINARY_DIR}/Noise/NoiseTargets.cmake"
        NAMESPACE Noise::
        )

install(EXPORT NoiseTargets
        FILE NoiseTargets.cmake
        NAMESPACE Noise::
        DESTINATION "${PACKAGE_CONFIG_INSTALL_DIR}"
        )
