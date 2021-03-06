find_package(Doxygen QUIET)

if(DOXYGEN)
    if(UNIX)
        execute_process(COMMAND date "+%b %d, %Y"
            OUTPUT_VARIABLE OMPL_RELEASE_DATE
            OUTPUT_STRIP_TRAILING_WHITESPACE)
    endif()
    configure_file("${CMAKE_CURRENT_SOURCE_DIR}/markdown/download.md.in"
        "${CMAKE_CURRENT_SOURCE_DIR}/markdown/download.md")
    configure_file("${CMAKE_CURRENT_SOURCE_DIR}/markdown/mainpage.md.in"
        "${CMAKE_CURRENT_SOURCE_DIR}/markdown/mainpage.md")
    file(READ "${CMAKE_SOURCE_DIR}/ompl/doc/images/ompl.svg" OMPLSVG)
    configure_file("${CMAKE_SOURCE_DIR}/ompl/doc/markdown/api_overview.md.in"
        "${CMAKE_SOURCE_DIR}/ompl/doc/markdown/api_overview.md")

    # add "doc" target if Doxygen is detected
    add_custom_target(doc
        COMMAND ./mkdocs.sh
        WORKING_DIRECTORY "${CMAKE_CURRENT_SOURCE_DIR}")

    # add "website" target
    add_custom_target(website
        COMMAND ./mkwebdocs.sh
        DEPENDS doc
        WORKING_DIRECTORY "${CMAKE_CURRENT_SOURCE_DIR}")
endif(DOXYGEN)

install(FILES man/ompl_benchmark.1 man/ompl_app.1
    DESTINATION ${CMAKE_INSTALL_MANDIR}/man1 COMPONENT omplapp)
