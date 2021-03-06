project(libxml2)

cmake_minimum_required(VERSION 2.4.0)

enable_language(C ASM)

if(COMMAND cmake_policy)
	cmake_policy(SET CMP0003 NEW)
endif(COMMAND cmake_policy)

add_definitions(-D__APPLE__ -D__MACH__)
add_definitions(-DTARGET_OS_MAC=1)
add_definitions(-DHAVE_STDINT_H=1)
add_definitions(-D__APPLE__ -D__DYNAMIC__)
add_definitions(-D__ENVIRONMENT_MAC_OS_X_VERSION_MIN_REQUIRED__=1080)

set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -nostdinc -D__DARWIN_UNIX03 -fPIC -w -include ${CMAKE_CURRENT_SOURCE_DIR}/config.h")
set(CMAKE_SHARED_LINKER_FLAGS "${CMAKE_SHARED_LINKER_FLAGS} -nostdlib")

include_directories(${CMAKE_CURRENT_SOURCE_DIR}/include)
include_directories(${CMAKE_CURRENT_SOURCE_DIR})
include_directories(${CMAKE_CURRENT_BINARY_DIR}/include)
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/../icu/icuSources/common)

set(LIBXML_VERSION_NUMBER "2.9.0")
set(LIBXML_VERSION_NUMBER 20900)
set(LIBXML_VERSION_EXTRA "-darling")
set(WITH_TRIO 0)
set(WITH_THREAD_ALLOC 0)
set(WITH_MEM_DEBUG 0)
set(WITH_RUN_DEBUG 0)
set(WITH_LZMA 0)
set(WITH_THREADS 1)
set(WITH_TREE 1)
set(WITH_OUTPUT 1)
set(WITH_PUSH 1)
set(WITH_READER 1)
set(WITH_PATTERN 1)
set(WITH_WRITER 1)
set(WITH_SAX1 1)
set(WITH_FTP 1)
set(WITH_HTTP 1)
set(WITH_VALID 1)
set(WITH_HTML 1)
set(WITH_LEGACY 1)
set(WITH_C14N 1)
set(WITH_CATALOG 1)
set(WITH_DOCB 1)
set(WITH_XPATH 1)
set(WITH_XPTR 1)
set(WITH_XINCLUDE 1)
set(WITH_ICONV 0)
set(WITH_ICU 1)
set(WITH_ISO8859X 1)
set(WITH_DEBUG 1)
set(WITH_REGEXPS 1)
set(WITH_EXPR 1)
set(WITH_SCHEMAS 1)
set(WITH_SCHEMATRON 1)
set(WITH_MODULES 1)
set(WITH_ZLIB 1)

configure_file(include/libxml/xmlversion.h.in include/libxml/xmlversion.h)

set(xml2_SRCS
	buf.c
	c14n.c
	catalog.c
	chvalid.c
	debugXML.c
	dict.c
	DOCBparser.c
	encoding.c
	entities.c
	error.c
	globals.c
	hash.c
	HTMLparser.c
	HTMLtree.c
	legacy.c
	list.c
	nanoftp.c
	nanohttp.c
	parser.c
	parserInternals.c
	pattern.c
	relaxng.c
	#rngparser.c
	SAX2.c
	SAX.c
	schematron.c
	threads.c
	tree.c
	trio.c
	trionan.c
	triostr.c
	uri.c
	valid.c
	xinclude.c
	xlink.c
	xmlIO.c
	xmlmemory.c
	xmlmodule.c
	xmlreader.c
	xmlregexp.c
	xmlsave.c
	xmlschemas.c
	xmlschemastypes.c
	xmlstring.c
	xmlunicode.c
	xmlwriter.c
	xpath.c
	xpointer.c
	xzlib.c
)

set(DYLIB_INSTALL_NAME "/usr/lib/libxml2.2.dylib")
set(DYLIB_COMPAT_VERSION "10.0.0")
set(DYLIB_CURRENT_VERSION "10.9.0")
add_darling_library(xml2 SHARED ${xml2_SRCS})
target_link_libraries(xml2 system icucore)
set_target_properties(xml2 PROPERTIES OUTPUT_NAME "xml2.2")
make_fat(xml2)

install(TARGETS xml2 DESTINATION libexec/darling/usr/lib)
InstallSymlink("libxml2.2.dylib" "${CMAKE_INSTALL_PREFIX}/libexec/darling/usr/lib/libxml2.dylib")

install(FILES xml2-config.1 DESTINATION libexec/darling/usr/share/man/man1)
install(FILES libxml.3 DESTINATION libexec/darling/usr/share/man/man3)
