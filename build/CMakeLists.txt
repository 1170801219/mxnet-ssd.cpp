CMAKE_MINIMUM_REQUIRED(VERSION 2.8)

#-------------------- WHAT TO CONFIG ------------------------#
SET(PROJECT_NAME "ssd")	# The project name
#------------------------------------------------------------#


#-------------------- NO MIDIFICATION ------------------------#
PROJECT(${PROJECT_NAME})

cmake_policy(SET CMP0015 NEW)
FILE(GLOB ALL_HEADERS "../src/*.h*")
FILE(GLOB ALL_SOURCES "../src/*.c*")
INCLUDE_DIRECTORIES("../include")
LINK_DIRECTORIES("../mxnet/lib" "../OpenBLAS")

ADD_EXECUTABLE(${PROJECT_NAME} ${ALL_HEADERS} ${ALL_SOURCES})
TARGET_LINK_LIBRARIES( ${PROJECT_NAME} "mxnet_predict" "openblas" "pthread" "X11")

include(CheckCXXCompilerFlag)
CHECK_CXX_COMPILER_FLAG("-std=c++11" COMPILER_SUPPORTS_CXX11)
CHECK_CXX_COMPILER_FLAG("-std=c++0x" COMPILER_SUPPORTS_CXX0X)
if(COMPILER_SUPPORTS_CXX11)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")
elseif(COMPILER_SUPPORTS_CXX0X)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++0x")
else()
        message(STATUS "The compiler ${CMAKE_CXX_COMPILER} has no C++11 support. Please use a different C++ compiler.")
endif()
