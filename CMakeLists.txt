cmake_minimum_required(VERSION 3.5)
project(soil  LANGUAGES C  VERSION 0.1.1  )

include_directories(inc)

ADD_LIBRARY( soil STATIC
    src/image_helper.c
    src/stb_image_aug.c
    src/image_DXT.c
    src/SOIL.c
    )

if( 0 )
    ADD_LIBRARY( soil SHARED
        src/image_helper.c
        src/stb_image_aug.c
        src/image_DXT.c
        src/SOIL.c
        )

    target_link_libraries( soil   -lGL  -lGLU  -lOPENGL32   Advapi32.lib  )
endif()

#-------------------install  destation ---------------------------
#set( DESTINATION  "../install/")
set(CMAKE_INSTALL_PREFIX "../install/")
#          And to the end of the top-level CMakeLists.txt we add:
#install(TARGETS ${PROJECT_NAME} DESTINATION bin)
#install(FILES "${PROJECT_BINARY_DIR}/TutorialConfig.h"  DESTINATION include  )


if (1)
    #
    # Format the project using the `clang-format` target (i.e: cmake --build build --target clang-format)
    #

    #add_clang_format_target()

    #
    # Install library for easy downstream inclusion
    #

    include(GNUInstallDirs)
    install(
        TARGETS
        ${PROJECT_NAME}
        EXPORT
        ${PROJECT_NAME}Targets
        LIBRARY DESTINATION
        ${CMAKE_INSTALL_LIBDIR}
        RUNTIME DESTINATION
        ${CMAKE_INSTALL_BINDIR}
        ARCHIVE DESTINATION
        ${CMAKE_INSTALL_LIBDIR}
        INCLUDES DESTINATION
        include
        PUBLIC_HEADER DESTINATION
        include
        )

    install(
        EXPORT
        ${PROJECT_NAME}Targets
        FILE
        ${PROJECT_NAME}Targets.cmake
        NAMESPACE
        ${PROJECT_NAME}::
        DESTINATION
        ${CMAKE_INSTALL_LIBDIR}/cmake/${PROJECT_NAME}
        )

    #
    # Add version header
    #

    configure_file(
        ${CMAKE_CURRENT_LIST_DIR}/cmake/version.hpp.in
        include/${PROJECT_NAME_LOWERCASE}/version.hpp
        @ONLY
        )

    install(
        FILES
        ${CMAKE_CURRENT_BINARY_DIR}/include/${PROJECT_NAME_LOWERCASE}/version.hpp
        DESTINATION
        include/${PROJECT_NAME_LOWERCASE}
        )

    #
    # Install the `include` directory
    #

    #install(
    #    DIRECTORY
    ##    include/${PROJECT_NAME_LOWERCASE}
    #    BasicDef/${PROJECT_NAME_LOWERCASE}
    #     exportApi/${PROJECT_NAME_LOWERCASE}
    #     DESTINATION
    #    include
    #    )

    install(
        FILES
        #    include/${PROJECT_NAME_LOWERCASE}
        inc/SOIL/image_DXT.h
        inc/SOIL/image_helper.h
        inc/SOIL/SOIL.h
        inc/SOIL/stb_image_aug.h
        inc/SOIL/stbi_DDS_aug_c.h
        inc/SOIL/stbi_DDS_aug.h
        #  exportApi/${PROJECT_NAME_LOWERCASE}/api_vision.h
        #     exportApi/${PROJECT_NAME_LOWERCASE}
        DESTINATION
        include/
        )


    #verbose_message("Install targets succesfully build. Install with `cmake --build <build_directory> --target install --config <build_config>`.")

    #
    # Quick `ConfigVersion.cmake` creation
    #

    include(CMakePackageConfigHelpers)
    write_basic_package_version_file(
        ${PROJECT_NAME}ConfigVersion.cmake
        VERSION
        ${PROJECT_VERSION}
        COMPATIBILITY
        SameMajorVersion
        )

    configure_package_config_file(
        ${CMAKE_CURRENT_LIST_DIR}/cmake/${PROJECT_NAME}Config.cmake.in
        ${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}Config.cmake
        INSTALL_DESTINATION
        ${CMAKE_INSTALL_LIBDIR}/cmake/${PROJECT_NAME}
        )

    install(
        FILES
        ${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}Config.cmake
        ${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}ConfigVersion.cmake
        DESTINATION
        ${CMAKE_INSTALL_LIBDIR}/cmake/${PROJECT_NAME}
        )

    #
    # Generate export header if specified
    #

    if (${PROJECT_NAME}_GENERATE_EXPORT_HEADER)
        include(GenerateExportHeader)
        generate_export_header(${PROJECT_NAME})
        install(
            FILES
            ${PROJECT_BINARY_DIR}/${PROJECT_NAME_LOWERCASE}_export.h
            DESTINATION
            include
            )
        message(STATUS "Generated the export header `${PROJECT_NAME_LOWERCASE}_export.h` and installed it.")
    endif ()

    message(STATUS "Finished building requirements for installing the package.\n")

    #
    # Unit testing setup
    #
    if (0)
        if (${PROJECT_NAME}_ENABLE_UNIT_TESTING)
            enable_testing()
            message(STATUS "Build unit tests for the project. Tests should always be found in the test folder\n")
            add_subdirectory(test)
        endif ()
    endif ()


endif ()




