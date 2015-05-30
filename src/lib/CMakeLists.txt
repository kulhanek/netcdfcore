# ==============================================================================
# NetCDF CMake File
# ==============================================================================

INCLUDE_DIRECTORIES(cnetcdf SYSTEM)

# include subdirectories -------------------------------------------------------
ADD_SUBDIRECTORY(cnetcdf)
IF(FORTRAN_ENABLED)
    ADD_SUBDIRECTORY(fnetcdf)
ENDIF(FORTRAN_ENABLED)

