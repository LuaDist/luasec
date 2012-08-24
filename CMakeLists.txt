# Copyright (C) 2007-2012 LuaDist.
# Submitted by David Manura
# Redistribution and use of this file is allowed according to the terms of the MIT license.
# For details see the COPYRIGHT file distributed with LuaDist.
# Please note that the package source code is licensed under its own license.

project ( luasec C )
cmake_minimum_required ( VERSION 2.8 )
include ( cmake/dist.cmake )
include ( lua )

find_library ( OPENSSL_SSL_LIBRARIES NAMES ssl ssleay32 ssleay32MD )
find_library ( OPENSSL_CRYPTO_LIBRARIES NAMES crypto )
if ( NOT OPENSSL_SSL_LIBRARIES )
  message ( "Cannot find openssl." )
endif ( )

# Build modules.
if ( WIN32 AND NOT CYGWIN )
  set ( SOCKET_LIBRARY ws2_32 )
  set ( SOCKETC src/wsocket.c )
else ( )
  set ( SOCKETC src/usocket.c )
endif ( )

install_lua_module ( ssl.context ssl.core ALL_IN_ONE src/timeout.c src/buffer.c src/io.c 
  src/context.c src/ssl.c ${SOCKETC} LINK ${OPENSSL_SSL_LIBRARIES} ${OPENSSL_CRYPTO_LIBRARIES} 
  ${SOCKET_LIBRARY} )
install_lua_module ( ssl src/ssl.lua )
install_lua_module ( ssl.https src/https.lua )
install_example ( samples/ )
install_data ( LICENSE )

