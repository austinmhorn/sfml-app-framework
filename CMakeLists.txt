cmake_minimum_required(VERSION 3.21)

set( SFAF_MAJOR_VERSION 0 )
set( SFAF_MINOR_VERSION 1 )
set( SFAF_REVISION_VERSION 0 )
set( SFAF_VERSION ${SFAF_MAJOR_VERSION}.${SFAF_MINOR_VERSION}.${SFAF_REVISION_VERSION} )

project( SFAF LANGUAGES CXX VERSION ${SFAF_VERSION} )
set( TARGET ${PROJECT_NAME} )

set( CMAKE_CXX_STANDARD 17 )
set( CMAKE_CXX_STANDARD_REQUIRED ON )
set( CMAKE_CXX_EXTENSIONS OFF )

set( CMAKE_RUNTIME_OUTPUT_DIRECTORY ${PROJECT_SOURCE_DIR}/bin )

# Find OpenGL
find_package( OpenGL REQUIRED )

if( NOT OPENGL_FOUND )
    message("Missing OpenGL" FATAL_ERROR)
endif()

# Find SFML
if( NOT TARGET sfml-graphics )
    find_package( SFML 2.5 REQUIRED COMPONENTS graphics window system )
endif()

if ( NOT SFML_FOUND )
include(FetchContent)
FetchContent_Declare(
    SFML
    GIT_REPOSITORY https://github.com/SFML/SFML.git
    GIT_TAG 2.6.x)
FetchContent_MakeAvailable(SFML)
endif()

set( SOURCE_PATH "${PROJECT_SOURCE_DIR}/src/" )

file(GLOB_RECURSE HPPS "${SOURCE_PATH}/*.hpp")
file(GLOB_RECURSE CPPS "${SOURCE_PATH}/*.cpp")

add_executable(${TARGET} ${CPPS} ${HPPS})

target_link_libraries( ${TARGET} PRIVATE sfml-graphics sfml-window sfml-system sfml-system sfml-audio )


install(TARGETS ${TARGET}
        CONFIGURATIONS Release
        RUNTIME DESTINATION bin)
