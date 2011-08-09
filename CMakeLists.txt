# Copyright (C) 2007-2009 LuaDist.
# Submitted by David Manura
# Redistribution and use of this file is allowed according to the terms of the MIT license.
# For details see the COPYRIGHT file distributed with LuaDist.
# Please note that the package source code is licensed under its own license.

PROJECT(luasec C)
CMAKE_MINIMUM_REQUIRED(VERSION 2.6)
INCLUDE(dist.cmake)

FIND_LIBRARY(OPENSSL_SSL_LIBRARIES NAMES ssl ssleay32 ssleay32MD)
FIND_LIBRARY(OPENSSL_CRYPTO_LIBRARIES NAMES crypto)
IF(NOT OPENSSL_SSL_LIBRARIES)
	MESSAGE("Cannot find openssl.")
ENDIF()

# Build modules.
IF (WIN32 AND NOT CYGWIN)
        SET(SOCKET_LIBRARY ws2_32)
        SET(SOCKETC src/wsocket.c)
ELSE ()
        SET(SOCKETC src/usocket.c)
ENDIF (WIN32 AND NOT CYGWIN)

ADD_LUA_MODULE(ssl src/timeout.c src/buffer.c src/io.c src/context.c src/ssl.c ${SOCKETC})
TARGET_LINK_LIBRARIES(ssl ${OPENSSL_SSL_LIBRARIES} ${OPENSSL_CRYPTO_LIBRARIES} ${SOCKET_LIBRARY})

# Install all files and documentation
INSTALL (TARGETS ssl DESTINATION ${INSTALL_CMOD})
INSTALL (FILES src/ssl.lua DESTINATION ${INSTALL_LMOD})
INSTALL (FILES src/https.lua DESTINATION ${INSTALL_LMOD}/ssl)
INSTALL (DIRECTORY samples/ DESTINATION ${INSTALL_EXAMPLE})
INSTALL (FILES LICENSE DESTINATION ${INSTALL_DATA})
