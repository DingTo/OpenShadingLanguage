add_custom_command (OUTPUT ${CMAKE_CURRENT_BINARY_DIR}/stdosl.h
    COMMAND ${CMAKE_COMMAND} -E copy_if_different ${CMAKE_CURRENT_SOURCE_DIR}/stdosl.h ${CMAKE_CURRENT_BINARY_DIR}
    MAIN_DEPENDENCY ${CMAKE_CURRENT_SOURCE_DIR}/stdosl.h
    WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR})

file (GLOB shader_headers "*.h")

if (BUILD_TESTING)
    file (GLOB shader_source "*.osl")

    macro (osl_compile oslsrc objlist headers)
        # message (STATUS "OSL_COMPILE src=${oslsrc}")
        # message (STATUS "  src ${CMAKE_CURRENT_SOURCE_DIR}")
        # message (STATUS "  bin ${CMAKE_CURRENT_BINARY_DIR}")
        get_filename_component ( oslsrc_we ${oslsrc} NAME_WE )
        set ( osofile "${CMAKE_CURRENT_BINARY_DIR}/${oslsrc_we}.oso" )
        # message (STATUS "  osl output ${osofile}")

        set ( ${objlist} ${${objlist}} ${osofile} )
        # message (STATUS "  obj list now ${${objlist}}")

#        include_directories ( ${CMAKE_CURRENT_BINARY_DIR} )
#        include_directories ( ${CMAKE_CURRENT_SOURCE_DIR} )
        message (STATUS "cmd: ${CMAKE_CURRENT_BINARY_DIR}/../oslc/oslc ${oslsrc}")
        add_custom_command (OUTPUT ${osofile}
            COMMAND ${CMAKE_CURRENT_BINARY_DIR}/../oslc/oslc ${oslsrc}
            MAIN_DEPENDENCY ${oslsrc}
            DEPENDS ${${headers}} ${oslsrc} ${CMAKE_CURRENT_BINARY_DIR}/stdosl.h ${CMAKE_CURRENT_BINARY_DIR}/../oslc/oslc
            WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR})
    endmacro ()

    foreach (_shadername ${shader_source})
        osl_compile (${_shadername} shader_objs shader_headers)
    endforeach ()


    add_custom_target (shaders ALL
                       DEPENDS ${shader_objs}
                       SOURCES ${shader_source} ${shader_headers})
else()
    add_custom_target (shaders ALL
                       SOURCES ${shader_headers})
endif()

install (FILES ${shader_headers} ${shader_source} ${shader_objs}
         DESTINATION shaders)

