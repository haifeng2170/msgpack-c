IF (MSGPACK_USE_X3_PARSE)
    INCLUDE_DIRECTORIES (
        ../include
    )

    LIST (APPEND exec_PROGRAMS
        unpack.cpp
        parse.cpp
    )
    IF (Boost_CONTEXT_LIBRARY AND Boost_SYSTEM_LIBRARY AND CMAKE_THREAD_LIBS_INIT)
        LIST (APPEND with_boost_PROGRAMS
            stream_unpack.cpp
        )
    ENDIF ()
    FOREACH (source_file ${exec_PROGRAMS})
        GET_FILENAME_COMPONENT (source_file_we ${source_file} NAME_WE)
        ADD_EXECUTABLE (
            ${source_file_we}
            ${source_file}
        )
        IF ("${CMAKE_CXX_COMPILER_ID}" STREQUAL "Clang" OR "${CMAKE_CXX_COMPILER_ID}" STREQUAL "GNU")
            SET_PROPERTY (TARGET ${source_file_we} APPEND_STRING PROPERTY COMPILE_FLAGS "-Wall -Wextra -Werror -g ")
        ENDIF ()

        IF ("${CMAKE_CXX_COMPILER_ID}" STREQUAL "Clang")
            SET_PROPERTY (TARGET ${source_file_we} APPEND_STRING PROPERTY COMPILE_FLAGS " -Wno-mismatched-tags")
        ENDIF ()

        IF ("${CMAKE_CXX_COMPILER_ID}" STREQUAL "MSVC")
            IF (CMAKE_CXX_FLAGS MATCHES "/W[0-4]")
                STRING(REGEX REPLACE "/W[0-4]" "/W3 /WX" CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS}")
            ELSE ()
                SET(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} /W3 /WX")
            ENDIF ()
        ENDIF ()
    ENDFOREACH ()
    FOREACH (source_file ${with_boost_PROGRAMS})
        GET_FILENAME_COMPONENT (source_file_we ${source_file} NAME_WE)
        ADD_EXECUTABLE (
            ${source_file_we}
            ${source_file}
        )
        TARGET_LINK_LIBRARIES (${source_file_we}
            ${Boost_CONTEXT_LIBRARY}
            ${Boost_SYSTEM_LIBRARY}
            ${CMAKE_THREAD_LIBS_INIT}
        )
        IF ("${CMAKE_CXX_COMPILER_ID}" STREQUAL "Clang" OR "${CMAKE_CXX_COMPILER_ID}" STREQUAL "GNU")
            SET_PROPERTY (TARGET ${source_file_we} APPEND_STRING PROPERTY COMPILE_FLAGS "-Wall -Wextra -Werror -g ")
        ENDIF ()

        IF ("${CMAKE_CXX_COMPILER_ID}" STREQUAL "Clang")
            SET_PROPERTY (TARGET ${source_file_we} APPEND_STRING PROPERTY COMPILE_FLAGS " -Wno-mismatched-tags")
        ENDIF ()

        IF ("${CMAKE_CXX_COMPILER_ID}" STREQUAL "MSVC")
            IF (CMAKE_CXX_FLAGS MATCHES "/W[0-4]")
                STRING(REGEX REPLACE "/W[0-4]" "/W3 /WX" CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS}")
            ELSE ()
                SET(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} /W3 /WX")
            ENDIF ()
        ENDIF ()
    ENDFOREACH ()
ENDIF ()
