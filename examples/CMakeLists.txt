macro(noise_example_project name source_file)

    add_executable(${name}
            ${source_file}
            noiseutils.cpp
            noiseutils.h
            )

    target_link_libraries(${name}
            PRIVATE
            libnoise_static
            )

endmacro()

noise_example_project(ComplexPlanet complexplanet.cpp)
noise_example_project(TextureGranite texturegranite.cpp)
noise_example_project(TextureJade texturejade.cpp)
noise_example_project(TextureSky texturesky.cpp)
noise_example_project(TextureSlime textureslime.cpp)
noise_example_project(TextureWood texturewood.cpp)

install(TARGETS
        ComplexPlanet
        TextureGranite
        TextureJade
        TextureSky
        TextureSlime
        TextureWood
        DESTINATION ${PACKAGE_BINARY_INSTALL_DIR}
        )

# Example requires OpenGL and GLUT libraries - only build if available
find_package(OpenGL)
find_package(GLUT)

if(OPENGL_FOUND AND GLUT_FOUND)
    noise_example_project(Worms worms.cpp)
    target_link_libraries(Worms
            PRIVATE
            OpenGL::GL
            OpenGL::GLU
            GLUT::GLUT
            )

    install(TARGETS
            Worms
            DESTINATION ${PACKAGE_BINARY_INSTALL_DIR}
            )

else()
    message(STATUS "OpenGL and/or GLUT library not found, not building \"Worms\" example.")
endif()
