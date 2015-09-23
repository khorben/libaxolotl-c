cmake_minimum_required(VERSION 2.8.4)
set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${CMAKE_SOURCE_DIR}/CMakeModules/")
project(axolotl-c)

INCLUDE(CheckSymbolExists)

CHECK_SYMBOL_EXISTS(memset_s "string.h" HAVE_MEMSET_S)

IF(CMAKE_SYSTEM_NAME MATCHES "Windows")
	CHECK_SYMBOL_EXISTS(SecureZeroMemory "Windows.h;WinBase.h" HAVE_SECUREZEROMEMORY)
ENDIF(CMAKE_SYSTEM_NAME MATCHES "Windows")

IF(BUILD_TESTING)
	enable_testing()
ENDIF(BUILD_TESTING)

IF(CMAKE_COMPILER_IS_GNUCC OR CMAKE_C_COMPILER_ID MATCHES "Clang")
	SET(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -fmessage-length=0 -Wall -Wmissing-field-initializers -Wno-missing-braces -Wparentheses")
ENDIF(CMAKE_COMPILER_IS_GNUCC OR CMAKE_C_COMPILER_ID MATCHES "Clang")

IF(CMAKE_COMPILER_IS_GNUCC)
	SET(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -Wsign-compare -Wsign-conversion")
ENDIF(CMAKE_COMPILER_IS_GNUCC)

IF(CMAKE_C_COMPILER_ID MATCHES "Clang")
	SET(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -Wswitch -Wunused-variable -Wunused-value -Wshadow -Wint-conversion -Wpointer-sign -Wprotocol -Wshorten-64-to-32")
ENDIF(CMAKE_C_COMPILER_ID MATCHES "Clang")

IF(HAVE_MEMSET_S)
	SET(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -DHAVE_MEMSET_S=1")
ENDIF(HAVE_MEMSET_S)

add_subdirectory(src)

IF(BUILD_TESTING)
	add_subdirectory(tests)
ENDIF(BUILD_TESTING)