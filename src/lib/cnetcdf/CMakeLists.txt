# ==============================================================================
# NetCDF CMake File
# ==============================================================================

# generate genconfig.h ------------------------------------------------------------

INCLUDE(CheckIncludeFiles)
INCLUDE(CheckCSourceCompiles)
# ------------------------------------------------
CHECK_INCLUDE_FILES("sys/stat.h" HAVE_SYS_STAT_H)
# ------------------------------------------------
CHECK_C_SOURCE_COMPILES("
#include <stdio.h>
#include <sys/stat.h>
int main(int argc, char* argv[])
{
  struct stat sb;
  return(sb.st_blksize);
}
" HAVE_STRUCT_STAT_ST_BLKSIZE)
# ------------------------------------------------
CHECK_C_SOURCE_COMPILES("
#include <stdio.h>
#include <sys/stat.h>
int main(int argc, char* argv[])
{
  struct stat sb;
  return(sb.st_blksize);
}
" HAVE_ST_BLKSIZE)
# ------------------------------------------------
CONFIGURE_FILE(${CMAKE_CURRENT_SOURCE_DIR}/config-gen.h.in ${CMAKE_CURRENT_SOURCE_DIR}/config-gen.h)

# objects in library -----------------------------------------------------------
SET(CNETCDF_SRC
        attr.c
        dim.c
        error.c
       # ffio.c
        libvers.c
        nc.c
        ncx.c
        posixio.c
        putget.c
        string.c
       # t_nc.c
        utf8proc.c
        v1hpg.c
        v2i.c
        var.c
        )

# create static library --------------------------------------------------------
IF(LIBS_STATIC)
    ADD_LIBRARY(cnetcdf_static STATIC ${CNETCDF_SRC})
    SET_TARGET_PROPERTIES(cnetcdf_static PROPERTIES
                            OUTPUT_NAME cnetcdf
                            CLEAN_DIRECT_OUTPUT 1)
ENDIF(LIBS_STATIC)

# create shared library --------------------------------------------------------
IF(LIBS_SHARED)
    ADD_DEFINITIONS(-DNETCDF_BUILDING_DLL)
    ADD_LIBRARY(cnetcdf_shared SHARED ${CNETCDF_SRC})
    SET_TARGET_PROPERTIES(cnetcdf_shared PROPERTIES
                            OUTPUT_NAME cnetcdf
                            CLEAN_DIRECT_OUTPUT 1
                            VERSION ${NETCDF_VERSION})
    INSTALL(TARGETS cnetcdf_shared
            DESTINATION lib)
ENDIF(LIBS_SHARED)

