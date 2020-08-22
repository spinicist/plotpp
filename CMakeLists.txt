cmake_minimum_required(VERSION 3.17)

project(plotpp)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED on)

# Uncomment for optional debugging
include(CheckCXXCompilerFlag)
include(CMakeToolsHelpers OPTIONAL)
include(CMakePrintHelpers)
include( ${PROJECT_SOURCE_DIR}/CMake/BuildType.cmake )

find_package(Eigen3 CONFIG REQUIRED)
find_package(fmt CONFIG REQUIRED)
find_package(args CONFIG REQUIRED)
find_package(skia CONFIG REQUIRED)
find_library(CoreFoundation CoreFoundation)

add_executable(
    HelloWorld
    source/HelloWorld.cpp
)

target_link_libraries(HelloWorld PRIVATE
    skia::skia fmt::fmt Eigen3::Eigen ${CoreFoundation}
)

install(TARGETS HelloWorld RUNTIME DESTINATION bin)

# Documentation
option(BUILD_DOC "Build documentation" ON)
find_package(Doxygen)
if (DOXYGEN_FOUND)
    set( DOXYGEN_OUTPUT_DIRECTORY doc )
    set( DOXYGEN_COLLABORATION_GRAPH YES )
    set( DOXYGEN_EXTRACT_ALL YES )
    set( DOXYGEN_CLASS_DIAGRAMS YES )
    set( DOXYGEN_HIDE_UNDOC_RELATIONS NO )
    set( DOXYGEN_HAVE_DOT YES )
    set( DOXYGEN_CLASS_GRAPH YES )
    set( DOXYGEN_CALL_GRAPH YES )
    set( DOXYGEN_CALLER_GRAPH YES )
    set( DOXYGEN_COLLABORATION_GRAPH YES )
    set( DOXYGEN_BUILTIN_STL_SUPPORT YES )
    set( DOXYGEN_EXTRACT_PRIVATE YES )
    set( DOXYGEN_EXTRACT_PACKAGE YES )
    set( DOXYGEN_EXTRACT_STATIC YES )
    set( DOXYGEN_EXTRACT_LOCALMETHODS YES )
    set( DOXYGEN_UML_LOOK YES )
    set( DOXYGEN_UML_LIMIT_NUM_FIELDS 50 )
    set( DOXYGEN_TEMPLATE_RELATIONS YES )
    set( DOXYGEN_DOT_GRAPH_MAX_NODES 100 )
    set( DOXYGEN_MAX_DOT_GRAPH_DEPTH 0 )
    set( DOXYGEN_DOT_TRANSPARENT YES )
    doxygen_add_docs(doc ${CMAKE_CURRENT_SOURCE_DIR}/src)
else (DOXYGEN_FOUND)
    message("Doxygen required to build the documentation")
endif (DOXYGEN_FOUND)