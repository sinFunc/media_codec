cmake_minimum_required(VERSION 3.19)
project(media_codec)

set(CMAKE_CXX_STANDARD 11)

set(CMAKE_LIBRARY_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} -Wl,--no-as-needed")
set(CMAKE_POSITION_INDEPENDENT_CODE ON)

option(LIB_MODE "build a library" ON)
option(ENABLE_TESTER "run tester" OFF)


SET(CODEC_INC_DIR ${CMAKE_CURRENT_SOURCE_DIR})

AUX_SOURCE_DIRECTORY(${CMAKE_CURRENT_SOURCE_DIR} CODEC_SRCS)
SET(ALL_SRCS ${CODEC_SRCS})

#
#INCLUDE(checkIncludeFile)
#INCLUDE(checkFunctionExists)
#INCLUDE(checkSymbolExists)


function(build_lib)
    set(TARGET_STATIC "${PROJECT_NAME}")
    add_library(${TARGET_STATIC} STATIC ${ALL_SRCS})
    TARGET_INCLUDE_DIRECTORIES(${TARGET_STATIC} PUBLIC ${CODEC_INC_DIR}) #fix header files depend
endfunction(build_lib)


function(tester)
    MESSAGE(STATUS,"run tester")
    #add use other libs here
endfunction(tester)


#function(install_include)
#    SET(INCLUDE_INSTALL_DIR ${CMAKE_INSTALL_PREFIX}/include/${PROJECT_NAME})
#    MESSAGE(STATUS "include install directory: ${INCLUDE_INSTALL_DIR}")
#
#    FILE(GLOB HEADERS "${CODEC_INC_DIR}/*.h*")
#    SET(HEADER_LIST)
#    FOREACH(inc ${HEADERS})
#        message(STATUS ${inc})
#        LIST(APPEND HEADER_LIST ${inc})
#    ENDFOREACH()
#
#    INSTALL(FILES ${HEADER_LIST}
#            DESTINATION ${INCLUDE_INSTALL_DIR}
#            PERMISSIONS OWNER_READ OWNER_WRITE GROUP_READ GROUP_WRITE)
#endfunction(install_include)

IF(LIB_MODE)
    MESSAGE(STATUS,"enable library mode.")
    build_lib()
#    install_include()
    IF(ENABLE_TESTER)
        tester()
    ENDIF()
ELSE()
    MESSAGE(STATUS,"enable executive mode.")

    add_executable(media_codec main.cpp ${ALL_SRCS})
ENDIF()





