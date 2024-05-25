# copied from: https://github.com/Iunusov/libshout-CMAKE/blob/master/CMakeLists.txt
cmake_minimum_required(VERSION 3.20)

project(shout VERSION 1.0 LANGUAGES C)

# Define version numbers
set(LIBSHOUT_MAJOR 2)
set(LIBSHOUT_MINOR 4)
set(LIBSHOUT_MICRO 6)

set(CMAKE_CXX_STANDARD 98)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_INCLUDE_CURRENT_DIR ON)

option(SHOUT_WITH_SPEEX "WITH_SPEEX" OFF)
option(SHOUT_WITH_OPENSSL "WITH_OPENSSL" OFF)
option(SHOUT_WITH_THEORA "WITH_THEORA" OFF)
option(SHOUT_WITH_VorbisOgg "Compile with Vorbis and Ogg" ON)
option(SHOUT_WITHOUT_PTHREAD "WITHOUT_PTHREAD" OFF)
if(SHOUT_WITH_VorbisOgg)
  add_subdirectory(${PROJECT_SOURCE_DIR}/Thirdparties/VorbisOgg ${PROJECT_BINARY_DIR}/VorbisOgg) # it add ogg and vorbis.
  set(VorbisOgg_Libraries
    vorbis
    vorbisenc
    vorbisfile
    Ogg::ogg
  )
endif()

if(SHOUT_WITHOUT_PTHREAD)
  set(HAVE_PTHREAD True)
endif()

if("${CMAKE_SYSTEM_NAME}" STREQUAL "OpenBSD" OR "${CMAKE_SYSTEM_NAME}" STREQUAL "SunOS" OR "${CMAKE_SYSTEM_NAME}" STREQUAL "Darwin")
  # Do nothing specific for these systems
else()
  add_definitions(-D_XOPEN_SOURCE=600)
endif()

set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -Wall -ffast-math -fsigned-char")

## Inspired from /usr/share/autoconf/autoconf/c.m4
#foreach(KEYWORD "inline" "__inline__" "__inline")
#  if(NOT DEFINED C_INLINE)
##    try_compile(C_HAS_${KEYWORD} "${PROJECT_SOURCE_DIR}"
##      "${PROJECT_SOURCE_DIR}/test_inline.c"
##      COMPILE_DEFINITIONS "-Dinline=${KEYWORD}")
#    if(C_HAS_${KEYWORD})
#      set(C_INLINE TRUE)
#      add_definitions("-Dinline=${KEYWORD}")
#    endif(C_HAS_${KEYWORD})
#  endif(NOT DEFINED C_INLINE)
#endforeach(KEYWORD)
#if(NOT DEFINED C_INLINE)
#  add_definitions("-Dinline=")
#endif(NOT DEFINED C_INLINE)

configure_file(${PROJECT_SOURCE_DIR}/include/shout/shout.h.in ${PROJECT_SOURCE_DIR}/include/shout/shout.h)

include(CheckIncludeFiles)
include(CheckFunctionExists)
include(CheckIncludeFile)

CHECK_FUNCTION_EXISTS(endhostent HAVE_ENDHOSTENT)
if(HAVE_ENDHOSTENT)
  add_definitions(-DHAVE_ENDHOSTENT)
endif()

CHECK_FUNCTION_EXISTS(ftime HAVE_FTIME)
if(HAVE_FTIME)
  add_definitions(-DHAVE_FTIME)
endif()

CHECK_FUNCTION_EXISTS(getaddrinfo HAVE_GETADDRINFO)
if(HAVE_GETADDRINFO)
  add_definitions(-DHAVE_GETADDRINFO)
endif()

CHECK_FUNCTION_EXISTS(getnameinfo HAVE_GETNAMEINFO)
if(HAVE_GETNAMEINFO)
  add_definitions(-DHAVE_GETNAMEINFO)
endif()

CHECK_FUNCTION_EXISTS(gettimeofday HAVE_GETTIMEOFDAY)
if(HAVE_GETTIMEOFDAY)
  add_definitions(-DHAVE_GETTIMEOFDAY)
endif()


CHECK_FUNCTION_EXISTS(inet_aton HAVE_INET_ATON)
if(HAVE_INET_ATON)
  add_definitions(-DHAVE_INET_ATON)
endif()

CHECK_FUNCTION_EXISTS(inet_pton HAVE_INET_PTON)
if(HAVE_INET_PTON)
  add_definitions(-DHAVE_INET_PTON)
endif()

CHECK_INCLUDE_FILE(inttypes.h HAVE_INTTYPES_H)
if(HAVE_INTTYPES_H)
  add_definitions(-DHAVE_INTTYPES_H)
endif()

CHECK_INCLUDE_FILE(stdint.h HAVE_STDINT_H)
if(HAVE_STDINT_H)
  add_definitions(-DHAVE_STDINT_H)
endif()

CHECK_INCLUDE_FILE(memory.h HAVE_MEMORY_H)
if(HAVE_MEMORY_H)
  add_definitions(-DHAVE_MEMORY_H)
endif()

CHECK_FUNCTION_EXISTS(nanosleep HAVE_NANOSLEEP)
if(HAVE_NANOSLEEP)
  add_definitions(-DHAVE_NANOSLEEP)
endif()

CHECK_INCLUDE_FILE(pthread.h HAVE_PTHREAD)
if(HAVE_PTHREAD)
  add_definitions(-DHAVE_PTHREAD)
endif()

if(SHOUT_WITHOUT_PTHREAD)
  set(HAVE_PTHREAD NO)
endif()

if(NOT HAVE_PTHREAD)
  add_definitions (-DNO_THREAD)
endif()

CHECK_FUNCTION_EXISTS(pthread_spin_lock HAVE_PTHREAD_SPIN_LOCK)
if(HAVE_PTHREAD_SPIN_LOCK)
  add_definitions(-DHAVE_PTHREAD_SPIN_LOCK)
endif()

CHECK_FUNCTION_EXISTS(sethostent HAVE_SETHOSTENT)
if(HAVE_SETHOSTENT)
  add_definitions(-DHAVE_SETHOSTENT)
endif()

CHECK_INCLUDE_FILE(stdlib.h HAVE_STDLIB_H)
if(HAVE_STDLIB_H)
  add_definitions(-DHAVE_STDLIB_H)
endif()

CHECK_INCLUDE_FILE(strings.h HAVE_STRINGS_H)
if(HAVE_STRINGS_H)
  add_definitions(-DHAVE_STRINGS_H)
endif()

CHECK_INCLUDE_FILE(string.h HAVE_STRING_H)
if(HAVE_STRING_H)
  add_definitions(-DHAVE_STRING_H)
endif()

CHECK_INCLUDE_FILE(sys/select.h HAVE_SYS_SELECT_H)
if(HAVE_SYS_SELECT_H)
  add_definitions(-DHAVE_SYS_SELECT_H)
endif()

CHECK_INCLUDE_FILE(sys/socket.h HAVE_SYS_SOCKET_H)
if(HAVE_SYS_SOCKET_H)
  add_definitions(-DHAVE_SYS_SOCKET_H)
endif()

CHECK_INCLUDE_FILE(sys/stat.h HAVE_SYS_STAT_H)
if(HAVE_SYS_STAT_H)
  add_definitions(-DHAVE_SYS_STAT_H)
endif()

CHECK_INCLUDE_FILE(sys/timeb.h HAVE_SYS_TIMEB_H)
if(HAVE_SYS_TIMEB_H)
  add_definitions(-DHAVE_SYS_TIMEB_H)
endif()

CHECK_INCLUDE_FILE(sys/types.h HAVE_SYS_TYPES_H)
if(HAVE_SYS_TYPES_H)
  add_definitions(-DHAVE_SYS_TYPES_H)
endif()

CHECK_INCLUDE_FILE(sys/uio.h HAVE_SYS_UIO_H)
if(HAVE_SYS_UIO_H)
  add_definitions(-DHAVE_SYS_UIO_H)
endif()

CHECK_INCLUDE_FILE(unistd.h HAVE_UNISTD_H)
if(HAVE_UNISTD_H)
  add_definitions(-DHAVE_UNISTD_H)
endif()


CHECK_INCLUDE_FILE(winsock2.h HAVE_WINSOCK2_H)
if(HAVE_WINSOCK2_H)
  add_definitions(-DHAVE_WINSOCK2_H)
endif()

CHECK_FUNCTION_EXISTS(writev HAVE_WRITEV)
if(HAVE_WRITEV)
  add_definitions(-DHAVE_WRITEV)
endif()

if(HAVE_PTHREAD)
  aux_source_directory(${PROJECT_SOURCE_DIR}/src/common/thread SOURCE_LIB)
endif()

file(GLOB_RECURSE SHOUT_SRCS_LIB
  ${PROJECT_SOURCE_DIR}/src/common/net/*.c
  ${PROJECT_SOURCE_DIR}/src/common/httpp/*.c
  ${PROJECT_SOURCE_DIR}/src/common/avl/*.c
  ${PROJECT_SOURCE_DIR}/src/common/timing/*.c
  ${PROJECT_SOURCE_DIR}/src/*.c
)
file(GLOB_RECURSE SHOUT_HDRS_LIB
  ${PROJECT_SOURCE_DIR}/*.h
)

list(APPEND
  SOURCE_LIB

  ${SOURCE_LIB}
  ${SHOUT_HDRS_LIB}
  ${SHOUT_SRCS_LIB}
)

if (NOT SHOUT_WITH_OPENSSL)
  list(REMOVE_ITEM SOURCE_LIB "${PROJECT_SOURCE_DIR}/src/tls.c")
endif()

if(NOT SHOUT_WITH_SPEEX)
  list(REMOVE_ITEM SOURCE_LIB "${PROJECT_SOURCE_DIR}/src/codec_speex.c")
endif()

if(NOT SHOUT_WITH_THEORA)
  list(REMOVE_ITEM SOURCE_LIB "${PROJECT_SOURCE_DIR}/src/codec_theora.c")
endif()

if(NOT SHOUT_WITH_VorbisOgg)
  list(REMOVE_ITEM SOURCE_LIB "${PROJECT_SOURCE_DIR}/src/codec_opus.c")
  list(REMOVE_ITEM SOURCE_LIB "${PROJECT_SOURCE_DIR}/src/format_ogg.c")
  list(REMOVE_ITEM SOURCE_LIB "${PROJECT_SOURCE_DIR}/src/codec_vorbis.c")
endif()

list(REMOVE_ITEM SOURCE_LIB "${PROJECT_SOURCE_DIR}/src/common/httpp/test.c")
list(REMOVE_ITEM SOURCE_LIB "${PROJECT_SOURCE_DIR}/src/common/avl/test.c")
list(REMOVE_ITEM SOURCE_LIB "${PROJECT_SOURCE_DIR}/src/common/log/test.c")
list(REMOVE_ITEM SOURCE_LIB "${PROJECT_SOURCE_DIR}/src/common/net/test_resolver.c")

#if(NOT WIN32)
#  list(REMOVE_ITEM SOURCE_LIB "${PROJECT_SOURCE_DIR}/win32compat/gettimeofday.c")
#endif()

add_library(${PROJECT_NAME} STATIC ${SOURCE_LIB})
add_library(libshout ALIAS ${PROJECT_NAME})
add_library(icecast-libshout ALIAS ${PROJECT_NAME})

if(SHOUT_WITH_VorbisOgg)
  target_link_libraries(${PROJECT_NAME} PUBLIC ${VorbisOgg_Libraries})
endif()
target_compile_definitions(${PROJECT_NAME} PRIVATE
  VERSION="${LIBSHOUT_MAJOR}.${LIBSHOUT_MINOR}.${LIBSHOUT_MICRO}"
  LIBSHOUT_MAJOR=${LIBSHOUT_MAJOR}
  LIBSHOUT_MINOR=${LIBSHOUT_MINOR}
  LIBSHOUT_MICRO=${LIBSHOUT_MICRO}
  HAVE_GETTIMEOFDAY=1
  TIME_WITH_SYS_TIME

)
if(WIN32)
  target_compile_definitions(${PROJECT_NAME} PRIVATE strcasecmp=_stricmp)
endif()

target_include_directories(${PROJECT_NAME} PUBLIC
  ${PROJECT_SOURCE_DIR}/include
  ${PROJECT_SOURCE_DIR}/include
  ${PROJECT_SOURCE_DIR}/src
  ${PROJECT_SOURCE_DIR}/src/common
)

if(WIN32)
  target_include_directories(${PROJECT_NAME} PUBLIC ${PROJECT_SOURCE_DIR}/win32compat/shout)
endif()

if(SHOUT_WITH_SPEEX)
  target_link_libraries(${PROJECT_NAME} PUBLIC speex)
endif()

if(SHOUT_WITH_THEORA)
  target_link_libraries(${PROJECT_NAME} PUBLIC theora)
endif()

if(SHOUT_WITH_OPENSSL)
  target_link_libraries(${PROJECT_NAME} PUBLIC ssl)
endif()

if(WIN32)
  find_library(WSOCK32_LIBRARY wsock32)
  find_library(WS2_32_LIBRARY ws2_32)
  target_link_libraries(${PROJECT_NAME} PUBLIC wsock32 ws2_32)
endif()

if(NOT SHOUT_WITHOUT_PTHREAD)
  find_package(Threads REQUIRED)
  target_link_libraries(${PROJECT_NAME} PUBLIC Threads::Threads)
  target_compile_definitions(${PROJECT_NAME} PRIVATE SHOUT_THREADSAFE=1)
else()
  target_compile_definitions(${PROJECT_NAME} PRIVATE -DNO_THREAD=1)
endif()

#set_property(TARGET ${PROJECT_NAME} PROPERTY POSITION_INDEPENDENT_CODE ON)
