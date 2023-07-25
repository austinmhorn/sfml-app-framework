cmake_minimum_required(VERSION 3.21)

set( SFAF_MAJOR_VERSION 0 )
set( SFAF_MINOR_VERSION 1 )
set( SFAF_REVISION_VERSION 0 )
set( SFAF_VERSION ${SFAF_MAJOR_VERSION}.${SFAF_MINOR_VERSION}.${SFAF_REVISION_VERSION} )

project( SFAF LANGUAGES CXX VERSION ${SFAF_VERSION} )
set(TARGET ${PROJECT_NAME})

set( CMAKE_CXX_STANDARD 11 )
set( CMAKE_CXX_STANDARD_REQUIRED ON )
set( CMAKE_CXX_EXTENSIONS OFF )

# Find packages
find_package( OpenGL REQUIRED )

if( NOT TARGET sfml-graphics )
    find_package( SFML 2.5 REQUIRED COMPONENTS graphics window system )
    
    if ( NOT SFML_FOUND )
    include(FetchContent)
    FetchContent_Declare(
        SFML
        GIT_REPOSITORY https://github.com/SFML/SFML.git
        GIT_TAG 2.6.x)
    FetchContent_MakeAvailable(SFML)
    endif()
endif()

set( SOURCE_PATH "${PROJECT_SOURCE_DIR}/src/" )

file(GLOB_RECURSE HPPS "${INCLUDE_PATH}/*.hpp")
file(GLOB_RECURSE INLS "${INCLUDE_PATH}/*.inl")
file(GLOB_RECURSE CPPS "${SOURCE_PATH}/*.cpp")

add_library( ${TARGET} ${CPPS} ${INLS} ${HPPS} )

add_executable(${PROJECT_NAME} ${SOURCE_FILES})

install(TARGETS ${PROJECT_NAME}
        CONFIGURATIONS Release
        RUNTIME DESTINATION bin)
