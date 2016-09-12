# Overview
CDO([Climate Data Operator](https://code.zmaw.de/projects/cdo)) is a software that has many  command line tools to manipulate and analysis climate data and NWP model data. Building cdo with [Magics](https://software.ecmwf.int/wiki/display/MAGP/Magics) enables it to [produce plots](https://code.zmaw.de/projects/cdo/wiki/Tutorial#Plotting).<br/>
This article shows how to build cdo with NetCDF, GRIB, HDF5, and Magics from source code. You can install cdo via package managers(like apt, yum, etc). (I'm not sure which third party libraries are included.)

# Software List
* cdo (1.7.2)
 * NetCDF (4.3.3.1)
   * zlib (1.2.8)
    * szlib (2.1)
     * HDF-5 (1.8.17)
      * libcurl (7.40.0)
 * GRIB_api (1.17.0)
   * OpenJPEG (1.5.1)
    * Jasper (1.900.1)
 * HDF-5 (1.8.17)
 * PROJ.4 (4.8.0)
 * Magics (2.18.15)
   * python (2.7.x)
    * swig (3.0.10)
     * numpy (1.11.1)
      * pango (1.39.0)
      * NetCDF-C++ (4.2.1)
      * boost (1.61.0)
      * libgd (2.2.3)
      * GRIB-api (1.17.0)


# Preparation
### * Check compilers: c,  c++ and GNU make
```bash
cc --version
```
```bash
g++ --version
```
```bash
make --version
```
If any output of the above command shows `command not found`, be sure to install them.
<br> </br>

### * Prepare building environment
For example, I want to install all of the packages under 
>/home/hsushipei/software/

To make future commands shorter
```bash
export INSTALL_ROOT=/home/hsushipei/software/
```
**Suggestion**: In order to prevent getting messed with source code bundle, files for building, and the software itself, I suggest you creating directories to keep source code bundle and building respectively.
```bash
mkdir $INSTALL_ROOT/src_bundle $INSTALL_ROOT/build
```
* For downloaded  source code bundle(`*tar.`*): keep them in `src_bundle`
* For untared bundle(directories): keep them in `build`
* For software itself: you can specify location to `--prefix` following `./configure`. To keep simple, I choose to install at  `$INSTALL_ROOT`
<br> </br>

### * Make things easier: Using Conda
I strongly recommend installing [conda](http://conda.pydata.org/docs/), a package and development enviorment manager for python. Conda helps you quickly and easly install some of the packages.<br/>
Download [miniconda](http://conda.pydata.org/miniconda.html) for __x86_64 linux system__.
```bash
wget https://repo.continuum.io/miniconda/Miniconda2-latest-Linux-x86_64.sh 
```
To install conda,
```bash
bash Miniconda2-latest-Linux-x86_64.sh
```
After you approve the license terms of, let conda be installed in default location, i.e.
>/home/hsushipei/miniconda2

and let installer to prepend the miniconda2 install location to PATH.
Re-login and check for conda installation
```bash
which conda
```
and the output should be

>~/miniconda2/bin/conda

Make future commands shorter.
```bash
export CONDA_ROOT=/home/hsushipei/miniconda2/
```

# Start Building
CDO requires NetCDF, GRIB_api, HDF5, PROJ.4, and Magics, and both NetCDF and Magics needs some sub-packages. We have to build the dependencies (sub-packages) first and after that the main package can be built.<br/>
Notice that if a certain software is required by at least two or more softwares, we don't have to build it twice, for exmaple, HDF-5 is prerequisite for NetCDF and cdo. <br/>
<br></br>
Now, take a breath and let's start!

* [NetCDF](http://www.unidata.ucar.edu/software/netcdf/) (4.3.3.1)
 * [zlib](http://www.zlib.net/) (1.2.8)<br/>
 Download version 1.2.8.
 ```bash
  wget http://zlib.net/zlib-1.2.8.tar.gz 
  ```
  Extract zlib from source code bundle and build it.
  ```bash
 ./configure --prefix=$INSTALL_ROOT
  ```
  ```bash
  make all install
  ```
 
 * [szlib](https://www.hdfgroup.org/HDF5/release/obtain5.html) (2.1)<br/>
 To download, click on "External Libraries Used by HDF5" and click "SZIP Source Code".
 ```bash
 ./configure --prefix=$INSTALL_ROOT
  ```
  ```bash
  make all install
  ```
 * [HDF5](https://www.hdfgroup.org/HDF5/release/obtain5.html) (2.8.17) <br/>
 Download version 2.8.17
 ```bash
 wget http://www.hdfgroup.org/ftp/HDF5/current/src/hdf5-1.8.17.tar.gz
 ```
 ```bash
 ./configure --with-zlib=$INSTALL_ROOT --with-szlib=$INSTALL_ROOT --prefix=$INSTALL_ROOT
 ```
 ```bash
 make all install
 ```
 * [libcurl](https://curl.haxx.se/) (7.40.0)<br/>
 Download version 7.40.0
 ```bash
 wget http://curl.haxx.se/download/curl-7.40.0.tar.gz
 ```
 ```bash
 ./configure --prefix=$INSTALL_ROOT --with-zlib=$INSTALL_ROOT --with-pic
 ```
 ```bash
 make all install
 ```

 If dependencies of NetCDF are successfully built, let's build NetCDF-4.3.3.1<br/>
Download NetCDF version 4.3.3.1.
```bash
wget https://github.com/Unidata/netcdf-c/archive/v4.3.3.1.tar.gz
```
Build NetCDF with HDF5 support
```bash
./configure --with-hdf5=$INSTALL_ROOT --with-zlib=$INSTALL_ROOT --with-szlib=$INSTALL_ROOT --prefix=/usr/local --enable-netcdf-4
```
```bash
make all install
```
<br></br>

* [GRIB_api](https://software.ecmwf.int/wiki/display/GRIB/Home) (1.17.0)
 * [Jasper](http://www.ece.uvic.ca/~frodo/jasper/) (1.900.1)<br/>
 Download version 1.900.1
 ```bash
 ./configure CFLAGS=-fPIC –prefix=$INSTALL_ROOT
 ```
 ```bash
 make all install
 ```
 
For GRIB_api, click [here](https://software.ecmwf.int/wiki/display/GRIB/Releases) and download version 1.17.0
```bash
./configure CFLAGS=-fPIC --prefix=$INSTALL_ROOT --with-netcdf=$INSTALL_ROOT --with-jasper=$INSTALL_ROOT --disable-jpeg
```
```bash
make && make install
```
<br></br>

* HDF5 (1.8.17)<br/>
See instruction above
<br></br>

* [PROJ.4](http://trac.osgeo.org/proj/) (4.8.0)<br/>
Download version 4.8.0
```bash
wget http://download.osgeo.org/proj/proj-4.8.0.tar.gz
```
```bash
./configure CFLAGS=-fPIC –prefix=$INSTALL_ROOT --enable-static=yes --enable-shared=no
```
```bash
make && make install
```
<br></br>

* [Magics](https://software.ecmwf.int/wiki/display/MAGP/Magics) (2.18.15)
 * python (2.7.x)<br/>
 Python is a built-in software in linux. Its version should be at least 2.7
 ```bash
 python --version
 ```
 * [swig](http://www.swig.org/index.php) (3.0.10)<br/>
 Swig can be installed by conda with one command.
 ```bash
 conda install swig
 ```
 * [numpy](http://www.numpy.org/) (1.11.1)
 ```bash
 conda install numpy
 ```
 * [pango](http://www.pango.org/) (1.39.0)
 ```bash
 conda install pango
 ```
 * [NetCDF-C++](https://www.unidata.ucar.edu/downloads/netcdf/netcdf-cxx/index.jsp) (4.2.1)<br/>
 Download version 4.2.1
 ```bash
 https://github.com/Unidata/netcdf-cxx4/archive/v4.2.1.tar.gz
 ```
 Build NetCDF-c++
 ```bash
 ./configure --with-hdf5=$INSTALL_ROOT --with-zlib=$INSTALL_ROOT --with-szlib=$INSTALL_ROOT --prefix=/usr/local --enable-netcdf-4
 ```
 ```bash
 make && make install
 ```
  * [boost](http://www.boost.org/) (1.61.0)<br/>
  Click [here](https://sourceforge.net/projects/boost/files/boost/1.61.0/) and download boost version 1.61.0
  ```bash
  ./bootstrap.sh --with-python=$CONDA_ROOT/bin/python --prefix=$INSTALL_ROOT
  ```
  ```bash
  ./b2 --prefix=$INSTALL_ROOT install
  ```
 * [libgd](https://libgd.github.io/) (2.2.3)<br/>
 Download version 2.2.3
 ```bash
 wget https://github.com/libgd/libgd/archive/gd-2.2.3.tar.gz
 ```
 ```bash
 ./configure --prefix=$INSTALL_ROOT
 ```
 ```bash
 make && make install
 ```
 * GRIB-api (1.17.0)
See instruction above

If all dependencies are built, build magics.<br/>
From [here](https://software.ecmwf.int/wiki/display/MAGP/Releases), download magics version 2.18.15
Set environmental variable
```bash
export LDFLAGS=-L$INSTALL_ROOT/lib/
```
```bash
./configure CFLAGS=-fPIC --prefix=$INSTALL_ROOT --enable-python --enable-grib --enable-proj4 --enable-netcdf --with-boost=$INSTALL_ROOT --with-grib-api=$INSTALL_ROOT --with-proj4=$INSTALL_ROOT --with-netcdf=$INSTALL_ROOT
```
```bash
make && make install
```
<br></br>

Finally, we can build cdo now
Download cdo version 1.7.2
```bash
wget https://code.zmaw.de/attachments/download/12760/cdo-1.7.2.tar.gz
```
```bash
./configure --prefix=$INSTALL_ROOT  --with-szlib=$INSTALL_ROOT --with-netcdf=$INSTALL_ROOT --with-jasper=$INSTALL_ROOT --with-hdf5=$INSTALL_ROOT --with-grib_api=$INSTALL_ROOT --with-magics=$INSTALL_ROOT --with-proj=$INSTALL_ROOT
```
```bash
make && make install
```
<br></br>

# Verify CDO Installation
Now, Adding cdo to PATH and re-login allows you launch cdo from shell
```bash
export PATH=/home/hsushipei/software/bin:${PATH}
```
You can verify whether external support from third party libraries is correctly linked to cdo. To check, try command
```bash
cdo -V
```
The output shows 
Also, you can try some cdo command to see if cdo works well
```bash
cdo shaded olr.nc plot_olr
```
<br></br>

# Troubleshooting
* __NetCDF__ and NetCDF-c++<br/>
If you got error message like
> “configure: error: Can't find or link to the hdf5 library. Use --disable-netcdf-4, or see config.log for errors.”

 You should set enviromental variables `LDFLAGS` and `CPPFLAGS`
```bash
export LDFLAGS=-L$INSTALL_ROOT/lib/
export CPPFLAGS=-I$INSTALL_ROOT/include/
```
and try `./configure` again
<br></br>

* __magics__<br/>
1.If you got error message like
>Can't locate XML/Parser.pm in @INC (you may need to install the XML::Parser module) (@INC contains: /home/hsushipei/software/lib/perl5/site_perl/5.24.0/x86_64-linux /home/hsushipei/software/lib/perl5/site_perl/5.24.0 /home/hsushipei/software/lib/perl5/5.24.0/x86_64-linux /home/hsushipei/software/lib/perl5/5.24.0 .) at ../../tools/xml2cc_new.pl line 3.
BEGIN failed--compilation aborted at ../../tools/xml2cc_new.pl line 3.

 **Solution**:You have to install [expat](http://expat.sourceforge.net/) and XML parser, a perl module.
 * Install expat
 ```bash
 ./configure --prefix=$INSTALL_ROOT
 ```
 ```bash
 make && make install
 ```
 * Install expat
 ```bash
 wget https://cpan.metacpan.org/authors/id/T/TO/TODDR/XML-Parser-2.44.tar.gz
 ```
 ```bash
 perl ./Makefile.PL EXPATLIBPATH=$INSTALL_ROOT/lib EXPATINCPATH=$INSTALL_ROOT/include
 ```
 `EXPATLIBPATH`: path where `libexpat` located
 `EXPATINCPATH`: path where `expat.h` located
 ```bash
 make && make install
 ```
 
 2.If you got error message like
>  No openjpeg (JPEG 2000) library could be found!
Your Magics++ programs should work nevertheless as long as
your GribAPI installed does NOT require it! Compile GribAPI
without openjpeg support.

 **Solution**: The version of openjpeg must less than or equal to 1.5.1

 3.If you got error message like
> configure: error: could not successfully compile with Boost::Math package

 You should set CPPFLAGS to tell compiler where the headers are.
```bash
export CPPFLAGS=-I/home/hsushipei/software/include/
```
# Known Issue
