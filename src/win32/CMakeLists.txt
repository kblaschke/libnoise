if(CMAKE_SYSTEM_NAME STREQUAL "Windows")
    add_library(libnoise_win32_dll OBJECT
            dllmain.cpp
            noise.rc
            resource.h
            )
endif()
