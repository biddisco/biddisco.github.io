# Compilation
## Daint
### ParaView
Generate list of plugin setting from cmake cache file
```
grep "PARAVIEW_BUILD_PLUGIN[^-]*:" CMakeCache.txt | sed 's/^/ -D/g' | sed 's/$/ \\/g' | sed 's/FALSE/OFF/g'
```
```
# modules we want 
module load cudatoolkit
module unload PrgEnv-cray
module load PrgEnv-gnu
module swap craype/2.2.0
module swap cray-libsci/13.0.1
module swap cray-mpich/7.0.3
module load cmake
module load viz

# compiler wrapper makes MPI easier, CMake will pick this up
export CC=/opt/cray/craype/default/bin/cc
export CXX=/opt/cray/craype/default/bin/CC

cmake \
 -DCMAKE_BUILD_TYPE=Debug \
 -DPARAVIEW_ENABLE_PYTHON:BOOL=OFF \
 -DPARAVIEW_USE_MPI:BOOL=ON \
 -DPARAVIEW_BUILD_QT_GUI:BOOL=OFF \
 -DBUILD_TESTING:BOOL=OFF \
 -DOPENGL_INCLUDE_DIR:PATH=/opt/cray/nvidia/default/include \
 -DOPENGL_gl_LIBRARY:FILEPATH=/opt/cray/nvidia/default/lib64/libGL.so \
 -DPARAVIEW_BUILD_PLUGIN_AdiosReader:BOOL=OFF \
 -DPARAVIEW_BUILD_PLUGIN_AnalyzeNIfTIIO:BOOL=OFF \
 -DPARAVIEW_BUILD_PLUGIN_ArrowGlyph:BOOL=OFF \
 -DPARAVIEW_BUILD_PLUGIN_CGNSReader:BOOL=OFF \
 -DPARAVIEW_BUILD_PLUGIN_EyeDomeLighting:BOOL=OFF \
 -DPARAVIEW_BUILD_PLUGIN_ForceTime:BOOL=OFF \
 -DPARAVIEW_BUILD_PLUGIN_GMVReader:BOOL=OFF \
 -DPARAVIEW_BUILD_PLUGIN_GeodesicMeasurement:BOOL=TRUE \
 -DPARAVIEW_BUILD_PLUGIN_H5PartReader:BOOL=OFF \
 -DPARAVIEW_BUILD_PLUGIN_InSituExodus:BOOL=OFF \
 -DPARAVIEW_BUILD_PLUGIN_MantaView:BOOL=OFF \
 -DPARAVIEW_BUILD_PLUGIN_Moments:BOOL=OFF \
 -DPARAVIEW_BUILD_PLUGIN_Nektar:BOOL=OFF \
 -DPARAVIEW_BUILD_PLUGIN_NonOrthogonalSource:BOOL=OFF \
 -DPARAVIEW_BUILD_PLUGIN_PacMan:BOOL=OFF \
 -DPARAVIEW_BUILD_PLUGIN_PointSprite:BOOL=OFF \
 -DPARAVIEW_BUILD_PLUGIN_PrismPlugin:BOOL=OFF \
 -DPARAVIEW_BUILD_PLUGIN_QuadView:BOOL=OFF \
 -DPARAVIEW_BUILD_PLUGIN_RGBZView:BOOL=OFF \
 -DPARAVIEW_BUILD_PLUGIN_SLACTools:BOOL=OFF \
 -DPARAVIEW_BUILD_PLUGIN_SciberQuestToolKit:BOOL=OFF \
 -DPARAVIEW_BUILD_PLUGIN_SierraPlotTools:BOOL=OFF \
 -DPARAVIEW_BUILD_PLUGIN_StreamingParticles:BOOL=OFF \
 -DPARAVIEW_BUILD_PLUGIN_SurfaceLIC:BOOL=OFF \
 -DPARAVIEW_BUILD_PLUGIN_UncertaintyRendering:BOOL=OFF \
 -DPARAVIEW_BUILD_PLUGIN_VaporPlugin:BOOL=OFF \
 /scratch/daint/biddisco/src/paraview
```
### BBP-pv
```
cmake \
 -DCMAKE_CXX_FLAGS=-std=c++11 \
 -DCUDA_NVCC_FLAGS=-std=c++11 \
 -DCUDA_PROPAGATE_HOST_FLAGS=OFF \
 -DCMAKE_OSX_ARCHITECTURES=x86_64 \
 -DBUILDYARD_DISABLED=ON \
 -DDISABLE_BUILDYARD=ON \
 -DBBP_SDK_JAVA=OFF \
 -DBBP_SDK_PYTHON=OFF \
 -DSUBPROJECT_BBPSDK:BOOL=ON \
 -DSUBPROJECT_BBPTestData:BOOL=OFF \
 -DSUBPROJECT_Brion:BOOL=ON \
 -DSUBPROJECT_Collage:BOOL=OFF \
 -DSUBPROJECT_DSM-Manager:BOOL=ON \
 -DSUBPROJECT_FlatBuffers:BOOL=OFF \
 -DSUBPROJECT_Lunchbox:BOOL=ON \
 -DSUBPROJECT_codash:BOOL=OFF \
 -DSUBPROJECT_dash:BOOL=OFF \
 -DSUBPROJECT_pv_bbp:BOOL=ON \
 -DSUBPROJECT_pv_cuda_piston:BOOL=ON \
 -DSUBPROJECT_pv_meshless:BOOL=ON \
 -DSUBPROJECT_pv_splotch:BOOL=ON \
 -DSUBPROJECT_pv_zoltan:BOOL=ON \
 -DSUBPROJECT_vmmlib:BOOL=ON \
 -DSUBPROJECT_zeq:BOOL=OFF \
 -DPV_CUDA_PISTON_USE_BOOST_TUPLE=OFF \
 -DPV_CUDA_PISTON_BUILD_REPRESENTATION=ON \
 -DPV_MESHLESS_USE_HDF5=ON \
 -DPV_MESHLESS_USE_OPENMP=ON \
 -DPV_SPLOTCH_DISABLE_READERS=ON \
 -DPV_SPLOTCH_USE_CUDA=ON \
 -DPV_SPLOTCH_USE_OPENMP=OFF \
 -DParaView_DIR=/scratch/daint/biddisco/build/pv4 \
 -DH5FDdsm_DIR:PATH=/users/biddisco/apps/daint/h5fddsm/share/cmake/h5fddsm \
 -DHDF5_DIR=/users/biddisco/apps/daint/hdf5_1_8_cmake/share/cmake/hdf5 \
 -DBOOST_ROOT=/apps/daint/boost/1.56.0/gnu_482/ \
 -DBoost_INCLUDE_DIR=/apps/daint/boost/1.56.0/gnu_482/include\
 -DBoost_LIBRARY_DIR=/apps/daint/boost/1.56.0/gnu_482/lib \
 /project/csvis/biddisco/src/bbp-pv
``` 
## Mac
### Boost
```
./bootstrap.sh --prefix=/Users/biddisco/apps/boost-1_56_0
/b2 cxxflags="-stdlib=libstdc++" linkflags="-stdlib=libstdc++" --prefix=/Users/biddisco/apps/boost-1_56_0 --layout=versioned threading=multi link=shared variant=release address-model=64 -j8 install
```
### hpx
```
cmake \
 -DCMAKE_BUILD_TYPE=Debug \
 -DCMAKE_INSTALL_PREFIX=/Users/biddisco/apps/hpx \
 -DHWLOC_ROOT=~/apps/hwloc-1.9.1/ \
 -DHWLOC_INCLUDE_DIR=~/apps/hwloc-1.9.1/lib/libhwloc.dylib \
 -DHWLOC_LIBRARY=~/apps/hwloc-1.9.1/lib/libhwloc.dylib \
 -DHPX_WITH_PARCELPORT_MPI=ON \
 -DHPX_WITH_PARCELPORT_TCP=OFF \
 -DHPX_BUILD_TESTS_EXTERNAL_BUILD=OFF \
 -DHPX_WITH_EXAMPLES=OFF \
 -DBOOST_ROOT=/Users/biddisco/apps/boost-1_57_0 \
 -DBoost_INCLUDE_DIR=/Users/biddisco/apps/boost-1_57_0/include/boost-1_57 \
 -DBoost_LIBRARY_DIR=/Users/biddisco/apps/boost-1_57_0/lib \
 -DBoost_COMPILER=-xgcc42 \
 ~/src/hpx
```

### spin-glass
```
cmake \
 -DCMAKE_BUILD_TYPE=Debug \
 -DHWLOC_ROOT=~/apps/hwloc-1.9.1/ \
 -DHWLOC_INCLUDE_DIR=~/apps/hwloc-1.9.1/lib/libhwloc.dylib \
 -DHWLOC_LIBRARY=~/apps/hwloc-1.9.1/lib/libhwloc.dylib \
 -DBOOST_ROOT=/Users/biddisco/apps/boost-1_57_0 \
 -DBoost_INCLUDE_DIR=/Users/biddisco/apps/boost-1_57_0/include/boost-1_57 \
 -DBoost_LIBRARY_DIR=/Users/biddisco/apps/boost-1_57_0/lib \
 -DBoost_COMPILER=-xgcc42 \
 -DHPX_DIR=/Users/biddisco/build/hpx/lib/cmake/hpx \
 ~/src/spin_glass_solver
```
### Spin glass master project
```
cmake \
-DCMAKE_BUILD_TYPE=Debug \
-DHWLOC_ROOT=~/apps/hwloc-1.9.1\
-DBOOST_ROOT=/Users/biddisco/apps/boost-1_57_0 \
-DBoost_INCLUDE_DIR=/Users/biddisco/apps/boost-1_57_0/include/boost-1_57 \ -DBoost_LIBRARY_DIR=/Users/biddisco/apps/boost-1_57_0/lib \
-DBoost_COMPILER=-xgcc42 \
-DHPX_DIR=/Users/biddisco/build/hpx/lib/cmake/hpx  \
~/src/spin_glass_solver
```

### ParaView
```
cmake 
 -DMPI_ROOT:STRING=/Users/biddisco/apps/mpich-3.1 \
 -DMPI_DIR:STRING=/Users/biddisco/apps/mpich-3.1 \
 -DMPI_C_INCLUDE_PATH:STRING=/Users/biddisco/apps/mpich-3.1/include \
 -DMPI_C_LIBRARIES:STRING=/Users/biddisco/apps/mpich-3.1/lib/libmpich.dylib \
 -DMPI_EXTRA_LIBRARY:STRING="/Users/biddisco/apps/mpich-3.1/lib/libmpi.dylib;/Users/biddisco/apps/mpich-3.1/lib/libmpl.dylib;/Users/biddisco/apps/mpich-3.1/lib/libopa.dylib" \
 -DMPI_CXX_INCLUDE_PATH:STRING=/Users/biddisco/apps/mpich-3.1/include \
 -DMPI_CXX_LIBRARIES:STRING="/Users/biddisco/apps/mpich-3.1/lib/libmpichcxx.dylib;/Users/biddisco/apps/mpich-3.1/lib/libpmpi.dylib" \
 -DMPI_C_COMPILER:STRING=/Users/biddisco/apps/mpich-3.1/bin/mpi-g++ \
 -DMPI_FOUND:BOOL=On \
 ~/src/paraview
```

### bbp-pv
```
# splotch needs -D_HAS_CPP0X for cuda compilation
cmake \
 -DCMAKE_CXX_FLAGS="-std=c++11" \
 -DBOOST_ROOT=/Users/biddisco/apps/boost-1_57_0/ \
 -DBoost_COMPILER=-xgcc42 \
 -DBUILDYARD_DISABLED=ON \
 -DCOMMON_LIBRARY_TYPE:STRING=STATIC \
 -DCMAKE_OSX_ARCHITECTURES=x86_64 \
 -DParaView_DIR=~/build/pv4 \
 -DHDF5_DIR=~/apps/hdf5_1_8_cmake/share/cmake/hdf5 \
 -DH5FDdsm_DIR=/Users/biddisco/apps/h5fddsm/share/cmake/h5fddsm/ \
 -DFLATBUFFERS_USE_CXX03_STDLIB=ON \
 -DPV_CUDA_PISTON_USE_BOOST_TUPLE=OFF \
 -DCUDA_NVCC_FLAGS="-gencode=arch=compute_20,code=compute_20 -std=c++11 -D_HAS_CPP0X" \
 -DCUDA_PROPAGATE_HOST_FLAGS:BOOL=OFF \
 -DICARUS_NO_LIBCXX:BOOL=ON \
 -DICARUS_USE_H5FDDSM:BOOL=ON \
 -DICARUS_USE_H5PART:BOOL=OFF \
 -DICARUS_USE_NETCDF:BOOL=OFF \
 -DSUBPROJECT_Collage:BOOL=OFF \
 -DSUBPROJECT_codash:BOOL=OFF \
 -DSUBPROJECT_pv_splotch:BOOL=OFF \
 ~/src/bbp-pv
```
```
cmake -G"Xcode" -DCMAKE_CXX_FLAGS="-std=c++11"  -DBOOST_ROOT=/Users/biddisco/apps/boost-1_57_0  -DBoost_COMPILER=-xgcc42 -DHPX_DIR=/Users/biddisco/build/hpx/lib/cmake/hpx -DHPX_IGNORE_COMPILER_COMPATIBILITY:BOOL=ON -DCMAKE_OSX_ARCHITECTURES=x86_64 ~/src/hpx-my
```
### Trlinos
```
cmake -DTrilinos_ASSERT_MISSING_PACKAGES=OFF -DBUILD_TESTING:BOOL=OFF -DTrilinos_ENABLE_Zoltan2:BOOL=ON -DTrilinos_ENABLE_Fortran:BOOL=OFF -DTPL_ENABLE_MPI:BOOL=ON -DTrilinos_ENABLE_ALL_OPTIONAL_PACKAGES=OFF ~/src/trilinos
make -j8
```
## BGAS

### boost (gcc)
```
./bootstrap.sh --with-toolset=gcc --prefix=/gpfs/bbp.cscs.ch/home/biddisco/apps/gcc-4.8.2/boost_1_55_0
./b2 cxxflags="-std=c++11" --prefix=/gpfs/bbp.cscs.ch/home/biddisco/apps/gcc-4.8.2/boost_1_56_0 --layout=versioned architecture=power threading=multi link=shared variant=release address-model=64 -j12 install
```

### hwloc
```
cd ../hwloc-1.8.1
make distclean
./configure \
  --host=powerpc64-bgq-linux \
  --prefix=/gpfs/bbp.cscs.ch/home/biddisco/apps/gcc-4.8.2/hwloc-1.8.1 \
  --disable-shared \
  --enable-static
#  CPPFLAGS='-I/bgsys/drivers/ppcfloor -I/bgsys/drivers/ppcfloor/spi/include/kernel/cnk/'
make install
```

### hpx
```
cmake \
-DCMAKE_CXX_FLAGS=-std=c++11 \
-DCMAKE_BUILD_TYPE=Release \
-DHPX_WITH_GENERIC_CONTEXT_COROUTINES:BOOL=OFF \
-DHPX_WITH_MALLOC=JEMALLOC \
-DJEMALLOC_INCLUDE_DIR:PATH=/gpfs/bbp.cscs.ch/home/biddisco/apps/gcc-4.8.2/jemalloc/include \
-DJEMALLOC_LIBRARY:FILEPATH=/gpfs/bbp.cscs.ch/home/biddisco/apps/gcc-4.8.2/jemalloc/lib/libjemalloc.so \
-DBoost_USE_STATIC_LIBS:BOOL=ON \
-DBOOST_ROOT=/gpfs/bbp.cscs.ch/home/biddisco/apps/gcc-4.8.2/boost_1_56_0 \
-DBOOST_INCLUDEDIR=/gpfs/bbp.cscs.ch/home/biddisco/apps/gcc-4.8.2/boost_1_56_0/include/boost-1_56 \
-DBOOST_LIBRARYDIR=/gpfs/bbp.cscs.ch/home/biddisco/apps/gcc-4.8.2/boost_1_56_0/lib \
-DBoost_NO_SYSTEM_PATHS=ON \
-DBoost_COMPILER=-gcc48 \
-DHWLOC_ROOT=/gpfs/bbp.cscs.ch/home/biddisco/apps/gcc-4.8.2/hwloc-1.8.1 \
-DHPX_WITH_PARCELPORT_VERBS:BOOL=ON \
-DHPX_PARCELPORT_VERBS_DEVICE:STRING=roq \
-DHPX_PARCELPORT_VERBS_IFNAME:STRING=ib0 \
-DHPX_PARCELPORT_VERBS_INTERFACE:STRING=roq0 \
-DHPX_PARCELPORT_VERBS_MAX_MEMORY_CHUNKS:STRING=100 \
-DHPX_PARCELPORT_VERBS_MEMORY_CHUNK_SIZE:STRING=4096 \
-DHPX_PARCELPORT_VERBS_MESSAGE_PAYLOAD:STRING=512 \
-DRDMAHELPER_WITH_LOGGING:BOOL=OFF \
-DHPX_WITH_THREAD_IDLE_RATES:BOOL=ON \
-DBoost_DEBUG=ON ~/src/hpx
```

###spin glass
```
cmake -DCMAKE_CXX_FLAGS="-std=c++11"
-DHPX_DIR=~/gcc/bgas/build/hpx/lib/cmake/hpx/
-DJEMALLOC_INCLUDE_DIR:PATH=/gpfs/bbp.cscs.ch/home/biddisco/apps/gcc-4.8.2/jemalloc/include
-DJEMALLOC_LIBRARY:FILEPATH=/gpfs/bbp.cscs.ch/home/biddisco/apps/gcc-4.8.2/jemalloc/lib/libjemalloc.so
-DBOOST_ROOT=/gpfs/bbp.cscs.ch/home/biddisco/apps/gcc-4.8.2/boost_1_56_0
-DBOOST_INCLUDEDIR=/gpfs/bbp.cscs.ch/home/biddisco/apps/gcc-4.8.2/boost_1_56_0/include/boost-1_56/
-DBOOST_LIBRARYDIR=/gpfs/bbp.cscs.ch/home/biddisco/apps/gcc-4.8.2/boost_1_56_0/lib
-DBoost_NO_SYSTEM_PATHS=ON -DBoost_COMPILER=-gcc48 -DBoost_DEBUG=ON
~/src/spin_glass_solver/
```

## brisi
### trilinos
```
cmake \
 -DTrilinos_ASSERT_MISSING_PACKAGES=OFF \
 -DTrilinos_ENABLE_ALL_OPTIONAL_PACKAGES=OFF \
 -DTrilinos_ENABLE_ALL_PACKAGES=OFF \
 -DTPL_BLAS_LIBRARIES=/opt/cray/libsci/13.0.3/gnu/49/x86_64/lib/libsci_gnu_49_mpi.so \
 -DTPL_LAPACK_LIBRARIES=/opt/cray/libsci/13.0.3/gnu/49/x86_64/lib/libsci_gnu_49_mpi.so \
 -DTPL_ENABLE_MPI=ON \
 -DTrilinos_ENABLE_Zoltan2=ON \
 /project/csvis/biddisco/src/trilinos
```
## Windows
### Boost
```
.\bootstrap.bat
b2 --prefix=c:\Boost\1_58_0_vs12 link=shared variant=release,debug architecture=x86 address-model=64 threading=multi install
```
## Monch
### MongoDB 
Without need to create compiler wrappers, use propagate setting tom pass ENV vars to scons
```
~/apps/monch/scons/bin/scons --propagate-shell-environment --prefix=/users/biddisco/apps/monch/mongodb-r3.03 --cxx=/apps/monch/gcc/4.9.0/bin/g++ --cc=/apps/monch/gcc/4.9.0/bin/gcc --config=force all
```
create init.sh with contents
```
module load gcc/4.9.0
export GCC_ROOT=/apps/monch/gcc/4.9.0
export LD_LIBRARY_PATH=/apps/monch/mpc/1.0.1/lib:/apps/monch/mpfr/3.1.2/lib:/apps/monch/gmp/5.1.2/lib:$LD_LIBRARY_PATH
```
create g++wrapper with contents
```
#!/bin/bash
# setup all environment I need, e.g. it sets GCC_ROOT and LD_LIBRARY_PATH
source /mnt/lnec/biddisco/build/init.sh
$GCC_ROOT/bin/g++ $@
```
create gccwrapper with contents
```
#!/bin/bash
# setup all environment I need, e.g. it sets GCC_ROOT and LD_LIBRARY_PATH
source /mnt/lnec/biddisco/build/init.sh
$GCC_ROOT/bin/gcc $@
```
compile MongoDB using scons
```
~/apps/monch/scons/bin/scons all --prefix=/users/biddisco/apps/monch/mongodb-r3.03 --cxx=/mnt/lnec/biddisco/build/g++wrapper --cc=/mnt/lnec/biddisco/build/gccwrapper --config=force
```
## Raspberry Pi (32bit builds)
Using Ubuntu virtual box, setup for cross compilation
```
export CXXFLAGS="-mcpu=cortex-a7 -mfpu=neon-vfpv4 -mfloat-abi=hard -D__arm__"
export   CFLAGS="-mcpu=cortex-a7 -mfpu=neon-vfpv4 -mfloat-abi=hard -D__arm__"
export PATH=/home/biddisco/pi/tools/arm-bcm2708/gcc-linaro-arm-linux-gnueabihf-raspbian-x64/bin/arm-linux-gnueabihf-g++:$PATH
alias gcc=/home/biddisco/pi/tools/arm-bcm2708/gcc-linaro-arm-linux-gnueabihf-raspbian-x64/bin/arm-linux-gnueabihf-gcc
alias g++=/home/biddisco/pi/tools/arm-bcm2708/gcc-linaro-arm-linux-gnueabihf-raspbian-x64/bin/arm-linux-gnueabihf-g++
```
### hwloc
```
./configure --host=arm-linux --prefix=/home/biddisco/apps/hwloc-1.10.1
```
### Boost
contents of ~/user-config.jam
```
using gcc : pi :
  /home/biddisco/pi/tools/arm-bcm2708/gcc-linaro-arm-linux-gnueabihf-raspbian-x64/bin/arm-linux-gnueabihf-g++ :
  <compileflags>"-D__arm__ -mcpu=cortex-a7 -mfpu=neon-vfpv4 -mfloat-abi=hard"
;
```
Build boost using 
```
./bootstrap.sh --prefix=/home/biddisco/apps/boost-1_58_0
./b2 -d+2 toolset=gcc-pi architecture=arm address-model=32 abi=aapcs binary-format=elf --debug-configuration --without-python -s NO_BZIP2=1 --layout=versioned threading=multi link=shared variant=release --prefix=/home/biddisco/apps/boost-1_58_0 -j8 install
```
### HPX
Toolchain file (work in progress)
```
set(CMAKE_SYSTEM_NAME Linux)
set(CMAKE_CROSSCOMPILING ON)

# specify the cross compiler
SET(CMAKE_C_COMPILER /home/biddisco/pi/tools/arm-bcm2708/gcc-linaro-arm-linux-gnueabihf-raspbian-x64/bin/arm-linux-gnueabihf-gcc)
SET(CMAKE_CXX_COMPILER /home/biddisco/pi/tools/arm-bcm2708/gcc-linaro-arm-linux-gnueabihf-raspbian-x64/bin/arm-linux-gnueabihf-g++)

# where is the target environment
SET(CMAKE_FIND_ROOT_PATH /home/biddisco/pi/tools/arm-bcm2708/gcc-linaro-arm-linux-gnueabihf-raspbian-x64)

# search for programs in the build host directories
set(CMAKE_FIND_ROOT_PATH_MODE_PROGRAM NEVER)
#set(CMAKE_FIND_ROOT_PATH_MODE_LIBRARY ONLY)
#set(CMAKE_FIND_ROOT_PATH_MODE_INCLUDE ONLY)
#set(CMAKE_FIND_ROOT_PATH_MODE_PACKAGE ONLY)

SET (CMAKE_CXX_FLAGS "-std=c++11 -mcpu=cortex-a7 -mfpu=neon-vfpv4 -mfloat-abi=hard --sysroot=/home/biddisco/pi/tools/arm-bcm2708/gcc-linaro-arm-linux-gnueabihf-raspbian-x64/arm-linux-gnueabihf/libc --sysroot=/home/biddisco/pi/tools/arm-bcm2708/gcc-linaro-arm-linux-gnueabihf-raspbian-x64/arm-linux-gnueabihf/libc")
SET (CMAKE_C_FLAGS "-mcpu=cortex-a7 -mfpu=neon-vfpv4 -mfloat-abi=hard --sysroot=/home/biddisco/pi/tools/arm-bcm2708/gcc-linaro-arm-linux-gnueabihf-raspbian-x64/arm-linux-gnueabihf/libc")
set(HPX_WITH_GENERIC_CONTEXT_COROUTINES ON CACHE BOOL "enable generic coroutines")
set(BOOST_UNDERLYING_PTHREAD_LIBRARY /home/biddisco/pi/tools/arm-bcm2708/gcc-linaro-arm-linux-gnueabihf-raspbian-x64/arm-linux-gnueabihf/libc/usr/lib/arm-linux-gnueabihf/libpthread.so)
```
And build with cmake
```
cmake --debug-try-compile \
-DCMAKE_BUILD_TYPE=Debug \
-DCMAKE_TOOLCHAIN_FILE=/home/biddisco/pi/CMakeRaspberryToolchain.cmake \
-DBOOST_ROOT=/home/biddisco/apps/boost-1_58_0 \
-DBoost_INCLUDE_DIR=/home/biddisco/apps/boost-1_58_0/include/boost-1_58 \
-DBOOST_LIBRARYDIR=/home/biddisco/apps/boost-1_58_0/lib \
-DHWLOC_ROOT=/home/biddisco/apps/hwloc-1.10.1 \
-DHPX_WITH_MALLOC=system \
-DCMAKE_CXX_FLAGS="-std=c++11 -mcpu=cortex-a7 -mfpu=neon-vfpv4 -mfloat-abi=hard --sysroot=/home/biddisco/pi/tools/arm-bcm2708/gcc-linaro-arm-linux-gnueabihf-raspbian-x64/arm-linux-gnueabihf/libc" \
-DBoost_COMPILER=-gcc \
-DBOOST_UNDERLYING_THREAD_LIBRARY=/home/biddisco/pi/tools/arm-bcm2708/gcc-linaro-arm-linux-gnueabihf-raspbian-x64/arm-linux-gnueabihf/libc/usr/lib/arm-linux-gnueabihf/libpthread.so \
-DHPX_WITH_GENERIC_CONTEXT_COROUTINES=ON \
~/src/hpx
```
