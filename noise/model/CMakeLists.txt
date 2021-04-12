add_library(libnoise_model OBJECT
        cylinder.cpp
        cylinder.h
        line.cpp
        line.h
        model.h
        plane.cpp
        plane.h
        sphere.cpp
        sphere.h
        )

target_include_directories(libnoise_model
        PUBLIC
        $<BUILD_INTERFACE:${CMAKE_SOURCE_DIR}>
        $<INSTALL_INTERFACE:${PACKAGE_INCLUDE_INSTALL_DIR}>
        )
