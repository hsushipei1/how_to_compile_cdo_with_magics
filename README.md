I will apologize in advance for my poor English and hope you can understand the whole article.
# Overview
CDO([Climate Data Operator](https://code.zmaw.de/projects/cdo)) is a software that has many  command line tools to manipulate and analysis climate data and NWP model data. Building cdo with [Magics](https://software.ecmwf.int/wiki/display/MAGP/Magics) enables it to [produce plots](https://code.zmaw.de/projects/cdo/wiki/Tutorial#Plotting).
This article shows how to build cdo with NetCDF, GRIB, HDF5, and Magics from source code. You can install cdo via package managers(like apt, yum, etc). (I'm not sure which third party libraries are included.)

# Software list
* cdo (1.7.2)
 * NetCDF (4.3.3.1)
   * zlib (1.2.8)
   * szlib (2.1)
   * HDF-5 (1.8.17)
   * libcurl (7.40.0)
 * GRIB_api (1.17.0)
 * HDF-5 (1.8.17)
 * PROJ.4 (4.8.0)
 * Magics (2.18.15)
   * python (2.7.x)
   * swig (3.0.10)
   * numpy (1.11.1)
   * pango (1.39.0)
   * NetCDF-C++ (4.2)
   * GRIB-api (1.17.0)



# Start building

# Troubleshooting

# Known issue
