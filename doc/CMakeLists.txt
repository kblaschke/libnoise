find_program(DOXYGEN_COMMAND doxygen)
find_program(PERL_COMMAND perl)

if(NOT DOXYGEN_COMMAND)
    message(STATUS "Doxygen command not found. To build documentation, please install Doxygen and add the \"doxygen\" executable location to CMAKE_PREFIX_PATH.")
    return()
endif()

if(NOT DOXYGEN_COMMAND)
    message(STATUS "Perl not found. To build documentation, please install Perl and add the \"perl\" executable location to CMAKE_PREFIX_PATH.")
    return()
endif()

set(DOC_CONFIG_PATH "${CMAKE_CURRENT_SOURCE_DIR}")
set(DOC_SOURCES_PATH "${CMAKE_SOURCE_DIR}/src")
set(DOC_OUTPUT_PATH "${CMAKE_CURRENT_BINARY_DIR}/html")

configure_file(Doxyfile.in "${CMAKE_CURRENT_BINARY_DIR}/Doxyfile" @ONLY)

add_custom_target(doc ALL
        COMMAND "${DOXYGEN_COMMAND}" "${CMAKE_CURRENT_BINARY_DIR}/Doxyfile"
        WORKING_DIRECTORY "${CMAKE_CURRENT_SOURCE_DIR}"
        )

install(DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/html/
        DESTINATION "${PACKAGE_DOCS_INSTALL_DIR}"
        )
