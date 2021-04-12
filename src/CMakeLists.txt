

add_subdirectory(model)
add_subdirectory(module)
add_subdirectory(win32)

add_library(libnoise_src OBJECT
        basictypes.h
        exception.h
        interp.h
        latlon.cpp
        latlon.h
        mathconsts.h
        misc.h
        noise.h
        noisegen.cpp
        noisegen.h
        vectortable.h
        )
