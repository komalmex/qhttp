cmake_minimum_required(VERSION 2.8.12 FATAL_ERROR)

set(PROJECT qhttp)
project(${PROJECT})

option (QHTTP_HAS_CLIENT "Build client part" ON)
OPTION(${PROJECT}_BUILD_SHARED "Build ${PROJECT} as shared libraries" ON)


set(CMAKE_INCLUDE_CURRENT_DIR ON)
add_definitions(-DQHTTP_MEMORY_LOG=0)


IF (WIN32 AND ${PROJECT}_BUILD_SHARED)
	add_definitions(-DQHTTP_EXPORT)
ENDIF ()

find_package (Qt5Core)
find_package (Qt5Network)

set(CMAKE_AUTOMOC ON)
set(CMAKE_AUTOUIC ON)

include_directories (${PROJECT_SOURCE_DIR}/../3rdparty)

SET(Project_sources
	${PROJECT_SOURCE_DIR}/../3rdparty/http-parser/http_parser.c
        qhttpabstracts.cpp 
    qhttpserverconnection.cpp 
    qhttpserverrequest.cpp 
    qhttpserverresponse.cpp 
    qhttpserver.cpp
)

SET(Project_headers
    ${PROJECT_SOURCE_DIR}/../3rdparty/http-parser/http_parser.h
    qhttpfwd.hpp 
    qhttpabstracts.hpp 
    qhttpserverconnection.hpp 
    qhttpserverrequest.hpp 
    qhttpserverresponse.hpp 
    qhttpserver.hpp
)

if(QHTTP_HAS_CLIENT)
SET(Project_sources
	${Project_sources}
        qhttpclientrequest.cpp 
        qhttpclientresponse.cpp 
        qhttpclient.cpp
)

SET(Project_headers
    ${Project_headers}
    qhttpclient.hpp 
        qhttpclientresponse.hpp 
        qhttpclientrequest.hpp
)
endif(QHTTP_HAS_CLIENT)

source_group ("Headers" FILES ${Project_headers})
source_group ("Sources" FILES ${Project_sources})

include_directories (${PROJECT_SOURCE_DIR}) 
include_directories (${PROJECT_BINARY_DIR}) 

IF(${PROJECT}_BUILD_SHARED)
    ADD_LIBRARY(${PROJECT} SHARED ${Project_sources} ${Project_headers})
ELSE()
    ADD_LIBRARY(${PROJECT} STATIC ${Project_sources} ${Project_headers})
ENDIF()

qt5_use_modules(${PROJECT} Core Network)


message("${PROJECT}_BUILD_SHARED:${${PROJECT}_BUILD_SHARED}")
#IF(${PROJECT}_BUILD_SHARED)
#    INSTALL(TARGETS ${PROJECT}
#        EXPORT flow-LibraryDependencies
#                RUNTIME DESTINATION bin
#                LIBRARY DESTINATION lib
#                ARCHIVE DESTINATION lib
#                )

#    INSTALL(FILES qtservice.h QtServiceBase QtServiceController DESTINATION ${QT_INCLUDE_DIR}/${PROJECT})

#    IF(UNIX)
#      INSTALL(FILES qtunixserversocket.h qtunixsocket.h DESTINATION ${QT_INCLUDE_DIR}/${PROJECT})
#    ENDIF()

#    CONFIGURE_FILE( ${CMAKE_CURRENT_SOURCE_DIR}/ProjectConfig.cmake.in
#            ${CMAKE_CURRENT_BINARY_DIR}/${PROJECT}-config.cmake @ONLY )

#    INSTALL(FILES ${PROJECT_BINARY_DIR}/${PROJECT}-config.cmake
#            DESTINATION share/cmake/${PROJECT} COMPONENT Development )

#    INSTALL(EXPORT flow-LibraryDependencies DESTINATION share/cmake/${PROJECT} COMPONENT Development)

#    set(QTSERVICE_INCLUDE_DIRS
#      ${CMAKE_CURRENT_SOURCE_DIR}
#      CACHE INTERNAL "${PROJECT} include directories"
#    )

#    set(QTSERVICE_LIBRARIES
#      ${PROJECT_NAME}
#      CACHE INTERNAL "${PROJECT} libraries"
#    )
#ENDIF()
