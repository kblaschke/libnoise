if(CMAKE_SYSTEM_NAME STREQUAL "Windows")
    add_library(libnoise_win32_dll OBJECT
            dllmain.cpp
            noise.rc
            resource.h
            )


    target_include_directories(libnoise_win32_dll
            PUBLIC
            $<BUILD_INTERFACE:${CMAKE_SOURCE_DIR}>
            $<INSTALL_INTERFACE:${PACKAGE_INCLUDE_INSTALL_DIR}>
            )

endif()
