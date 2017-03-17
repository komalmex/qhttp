cmake_minimum_required(VERSION 2.8.12 FATAL_ERROR)

set(PROJECT qhttp_p)
project(${PROJECT})

add_definitions(-DQHTTP_HAS_CLIENT)

if (MSVC)
        # Options for Visual Studio can be added here
elseif (CMAKE_COMPILER_IS_GNUCXX)
        set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11 -std=c++14 -Wall -pedantic -fmessage-length=0")
        # TODO: enable Werror once algorithm is ready
        #set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Werror")
elseif ("${CMAKE_CXX_COMPILER_ID}" STREQUAL "Clang")
        set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11 -std=c++14")
elseif ("${CMAKE_CXX_COMPILER_ID}" STREQUAL "Intel")
        set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11 -std=c++14")
else ()
        message ("Unknown compiler")
endif ()

message("CMAKE_CXX_FLAGS ${CMAKE_CXX_FLAGS}")

add_subdirectory(src)

