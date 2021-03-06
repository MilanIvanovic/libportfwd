PROJECT(libportfwd)
CMAKE_MINIMUM_REQUIRED(VERSION 2.6 FATAL_ERROR)
SET(CMAKE_VERBOSE_MAKEFILE ON)
#SET(CMAKE_INSTALL_PREFIX ".")

SET(MINIUPNP_DIR    "third-party/miniupnpc-1.4.20100609/")
SET(NATPMP_DIR      "third-party/libnatpmp-20100202")

SET(CMAKE_RUNTIME_OUTPUT_DIRECTORY "${CMAKE_CURRENT_SOURCE_DIR}")
SET(CMAKE_LIBRARY_OUTPUT_DIRECTORY "${CMAKE_CURRENT_SOURCE_DIR}")
SET(CMAKE_ARCHIVE_OUTPUT_DIRECTORY "${CMAKE_CURRENT_SOURCE_DIR}")
ADD_DEFINITIONS(-Wall -O2 -DNDEBUG)
IF(WIN32)
	ADD_DEFINITIONS(-DWIN32 -DMINIUPNP_EXPORTS )
ELSE()
	ADD_DEFINITIONS(-fPIC)
ENDIF()
INCLUDE_DIRECTORIES(${MINIUPNP_DIR} include)

ADD_LIBRARY(portfwd STATIC

        # the needed bits of miniupnpc (no python module, no tests, no cli)
        ${MINIUPNP_DIR}/connecthostport.c
        ${MINIUPNP_DIR}/igd_desc_parse.c
        ${MINIUPNP_DIR}/minisoap.c
        ${MINIUPNP_DIR}/minissdpc.c
        ${MINIUPNP_DIR}/miniupnpc.c
        ${MINIUPNP_DIR}/miniwget.c
        ${MINIUPNP_DIR}/minixml.c
        ${MINIUPNP_DIR}/minixmlvalid.c
        ${MINIUPNP_DIR}/upnpc.c
        ${MINIUPNP_DIR}/upnpcommands.c
        ${MINIUPNP_DIR}/upnperrors.c
        ${MINIUPNP_DIR}/upnpreplyparse.c

        # Our wrapper:
        src/portfwd.cpp
)

IF(WIN32)
	TARGET_LINK_LIBRARIES( portfwd "ws2_32.dll" )
ENDIF()

#ADD_EXECUTABLE(portfwd-demo
#               src/main.cpp
#              )
#TARGET_LINK_LIBRARIES(portfwd-demo portfwd)

#INSTALL(TARGETS portfwd ARCHIVE DESTINATION lib)
#INSTALL(TARGETS portfwd-demo RUNTIME DESTINATION bin)
#INSTALL(DIRECTORY include/portfwd DESTINATION include PATTERN "*~" EXCLUDE)
