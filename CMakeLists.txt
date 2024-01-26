cmake_minimum_required(VERSION 3.12)

project(freetype)

file(GLOB_RECURSE FREETYPE_GLOB_SOURCES
	"freetype/include/freetype/*.h"
	"freetype/include/freetype/config/*.h"
	"freetype/include/freetype/internal/*.h"
)

set(FREETYPE_SOURCES
	freetype/include/ft2build.h
	freetype/src/autofit/autofit.c
	freetype/src/base/ftbase.c
	freetype/src/base/ftbbox.c
	freetype/src/base/ftbdf.c
	freetype/src/base/ftbitmap.c
	freetype/src/base/ftcid.c
	freetype/src/base/ftdebug.c
	freetype/src/base/ftfstype.c
	freetype/src/base/ftgasp.c
	freetype/src/base/ftglyph.c
	freetype/src/base/ftgxval.c
	freetype/src/base/ftinit.c
	freetype/src/base/ftmm.c
	freetype/src/base/ftotval.c
	freetype/src/base/ftpatent.c
	freetype/src/base/ftpfr.c
	freetype/src/base/ftstroke.c
	freetype/src/base/ftsynth.c
	freetype/src/base/ftsystem.c
	freetype/src/base/fttype1.c
	freetype/src/base/ftwinfnt.c
	freetype/src/bdf/bdf.c
	freetype/src/bzip2/ftbzip2.c
	freetype/src/cache/ftcache.c
	freetype/src/cff/cff.c
	freetype/src/cid/type1cid.c
	freetype/src/gzip/ftgzip.c
	freetype/src/lzw/ftlzw.c
	freetype/src/pcf/pcf.c
	freetype/src/pfr/pfr.c
	freetype/src/psaux/psaux.c
	freetype/src/pshinter/pshinter.c
	freetype/src/psnames/psnames.c
	freetype/src/raster/raster.c
	freetype/src/sdf/sdf.c
	freetype/src/sfnt/sfnt.c
	freetype/src/smooth/smooth.c
	freetype/src/truetype/truetype.c
	freetype/src/type1/type1.c
	freetype/src/type42/type42.c
	freetype/src/winfonts/winfnt.c
)

list(APPEND FREETYPE_SOURCES ${FREETYPE_GLOB_SOURCES})

add_library(freetype STATIC ${FREETYPE_SOURCES})

target_include_directories(freetype PRIVATE
    freetype/include
)

target_compile_definitions(freetype PRIVATE
    FT2_BUILD_LIBRARY
    _CRT_SECURE_NO_WARNINGS
    _CRT_NONSTDC_NO_WARNINGS
)

if(WIN32)
    target_compile_options(freetype PRIVATE "/MP")
    target_compile_definitions(freetype PRIVATE
        UNICODE
        _UNICODE
    )
endif()

project(msdfgen)

file(GLOB_RECURSE MSDFGEN_FILES
	"core/*.h"
	"core/*.hpp"
	"core/*.cpp"
	"ext/*.h"
	"ext/*.hpp"
	"ext/*.cpp"
	"lib/*.cpp"
	"include/*.h"
)

add_library(msdfgen STATIC ${MSDFGEN_FILES})

target_include_directories(msdfgen PRIVATE
    include
    freetype/include
)

target_compile_definitions(msdfgen PRIVATE
    MSDFGEN_USE_CPP11
)

target_link_libraries(msdfgen PRIVATE
    freetype
)

if(CMAKE_SYSTEM_NAME STREQUAL "Windows")
    target_compile_options(msdfgen PRIVATE
        /W3
    )
endif()
