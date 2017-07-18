# ==============================================================================
# NetCDF CMake File
# ==============================================================================

# objects in library -----------------------------------------------------------
SET(FNETCDF_SRC
        fort-attio.c
        fort-control.c
        fort-dim.c
        fort-genatt.c
        fort-geninq.c
        fort-genvar.c
        fort-lib.c
        fort-misc.c
        fort-var1io.c
        fort-varaio.c
        fort-vario.c
        fort-varmio.c
        fort-varsio.c
        netcdf.f90
        typeSizes.f90
        )

# create static library --------------------------------------------------------
IF(LIBS_STATIC)
    ADD_LIBRARY(fnetcdf_static STATIC ${FNETCDF_SRC})
    TARGET_LINK_LIBRARIES(fnetcdf_static
                    cnetcdf_static
                    )
    SET_TARGET_PROPERTIES(fnetcdf_static PROPERTIES
                            OUTPUT_NAME fnetcdf
                            CLEAN_DIRECT_OUTPUT 1)
ENDIF(LIBS_STATIC)

# create shared library --------------------------------------------------------
IF(LIBS_SHARED)
    ADD_LIBRARY(fnetcdf_shared SHARED ${FNETCDF_SRC})
    SET_TARGET_PROPERTIES(fnetcdf_shared PROPERTIES
                            OUTPUT_NAME fnetcdf
                            CLEAN_DIRECT_OUTPUT 1
                            VERSION ${NETCDF_VERSION})    
    
    ADD_DEPENDENCIES(fnetcdf_shared cnetcdf_shared)
    TARGET_LINK_LIBRARIES(fnetcdf_shared
                    cnetcdf_shared
                    )
    
    INSTALL(TARGETS fnetcdf_shared
            DESTINATION lib)
ENDIF(LIBS_SHARED)

