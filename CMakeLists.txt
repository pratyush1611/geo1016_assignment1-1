cmake_minimum_required(VERSION 3.1)

get_filename_component(PROJECT_NAME ${CMAKE_CURRENT_SOURCE_DIR} NAME)
project(${PROJECT_NAME})

# One shouldn't generate the BUILD project directly in the SOURCES folder!
if ( ${CMAKE_CURRENT_BINARY_DIR} STREQUAL ${CMAKE_CURRENT_SOURCE_DIR} )
        if ( NOT SAME_BUILD_AND_SOURCE_FOLDER_WARNING_ALREADY_ISSUED )
                message(FATAL_ERROR
                    "It is not advised to BUILD the binaries directly in the SOURCE folder!\n If
                    you want to proceed with this option, just CONFIGURE the project once again" )
                set( SAME_BUILD_AND_SOURCE_FOLDER_WARNING_ALREADY_ISSUED TRUE )
        endif()
endif()

################################################################################

#math(EXPR EASY3D_OS_BITS "8 * ${CMAKE_SIZEOF_VOID_P}")
#message(STATUS "Easy3d: I'am woking on ${EASY3D_OS_BITS}-bit ${CMAKE_SYSTEM_NAME}")
if(CMAKE_SIZEOF_VOID_P EQUAL 8)
    message(STATUS "Easy3D: I'am working on a 64-bit ${CMAKE_SYSTEM_NAME}")
else()
    message(STATUS "Easy3D: I'am working on a 32-bit ${CMAKE_SYSTEM_NAME}")
endif()

# specify the C++ standard
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED True)

################################################################################

# Detects whether this is a top-level project
if(CMAKE_SOURCE_DIR STREQUAL CMAKE_CURRENT_SOURCE_DIR)
	set(EASY3D_TOPLEVEL_PROJECT ON)
else()
	set(EASY3D_TOPLEVEL_PROJECT OFF)
endif()

set_property(GLOBAL PROPERTY USE_FOLDERS ON)

################################################################################

### Configuration
set(EASY3D_ROOT         ${CMAKE_CURRENT_LIST_DIR})
set(EASY3D_THIRD_PARTY  ${EASY3D_ROOT}/3rd_party)

set(EASY3D_INCLUDE_DIR  ${EASY3D_ROOT} ${CMAKE_CURRENT_BINARY_DIR})
set(EASY3D_SOURCE_DIR   ${EASY3D_ROOT})

### conditionally compile certain modules depending on libraries found on the system
list(APPEND CMAKE_MODULE_PATH ${CMAKE_CURRENT_LIST_DIR}/cmake)

################################################################################

set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)
set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)

################################################################################

add_subdirectory(3rd_party)
add_subdirectory(easy3d)
add_subdirectory(Calibration)

# hide some variables that might be set in 3rd_party libraries
mark_as_advanced(FORCE BUILD_SHARED_LIBS)
mark_as_advanced(FORCE BUILD_TESTING)