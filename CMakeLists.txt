# Copyright (C) 2007-2009 LuaDist.
# Submitted by David Manura
# Redistribution and use of this file is allowed according to the terms of the MIT license.
# For details see the COPYRIGHT file distributed with LuaDist.
# Please note that the package source code is licensed under its own license.

PROJECT(luasocket C)
CMAKE_MINIMUM_REQUIRED(VERSION 2.8)
INCLUDE(dist.cmake)

INCLUDE(FindOpenSSL)
FIND_LIBRARY(CRYPTO_LIBRARY NAMES crypto)

# Build modules.
IF (WIN32 AND NOT CYGWIN)
        SET(SOCKETC src/wsocket.c)
ELSE ()
        SET(SOCKETC src/usocket.c)
ENDIF (WIN32 AND NOT CYGWIN)
IF (WIN32 AND NOT CYGWIN)
        SET(SOCKET_LIBRARY ws2_32)
ENDIF (WIN32 AND NOT CYGWIN)

ADD_LUA_MODULE(ssl src/timeout.c src/buffer.c src/io.c src/context.c src/ssl.c ${SOCKETC})
TARGET_LINK_LIBRARIES(ssl ${OPENSSL_LIBRARIES} ${CRYPTO_LIBRARY} ${SOCKET_LIBRARY})

# Install all files and documentation
INSTALL (TARGETS ssl DESTINATION ${INSTALL_CMOD})
INSTALL (FILES src/ssl.lua DESTINATION ${INSTALL_LMOD})
INSTALL (FILES src/https.lua DESTINATION ${INSTALL_LMOD}/ssl)
INSTALL (DIRECTORY samples/ DESTINATION ${INSTALL_EXAMPLE})
INSTALL (FILES LICENSE DESTINATION ${INSTALL_DATA})
