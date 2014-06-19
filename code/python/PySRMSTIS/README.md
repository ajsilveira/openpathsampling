so far contains only an OpenMM Tutorial for python from ASJS Mey

if you try to install OpenMM on a Mac using 10.9  and especially a new MacBook Pro and want to use CUDA the following installation guide might help

## Installation of OpenMM under Mac OS X 10.9

### General Prerequisites
1. For the installation of required packages a package manager is useful, e.g. MacPorts or Homebrew.
Download the installers under

```http://www.macports.org/install.php```

or using

```ruby -e "$(curl -fsSL https://raw.github.com/Homebrew/homebrew/go/install)"```

2. Make sure that the latest XCode is installed from the App-Store and also the Command-Line Tools
```xcode-select --install```

### Installation Procedure

1. create new directoy where you would like to install openmm, like "openmm"

2. inside of it clone using the ssh github
```git clone git@github.com:SimTk/openmm.git```

3. rename openmm to openmm.simtk to make sure that you always have the original version available
```mv openmm/ openmm.simtk```

4. install doxygen using
```sudo port install doxygen```

5. install swig using
  ```sudo port install swig```
  This can also be installed using homebrew with
  ```sudo brew install swig```

6. install fftw library for discrete fast fourier transformation
```sudo port install fftw-3-single```

7. run CMake to create the Makefile configuration CMakeCache.txt
```cmake .```

8. edit the CMakeCache.txt using your favourite text editor and edit the following parts

8.1 set the processor architecture to 64-bit by removing 32-bit if your installed fftw is also set to 64-bit

```
//The processor architectures to build for
CMAKE_OSX_ARCHITECTURES:STRING=x86_64
```

8.2. set the path for the fftw library 

```
//Path to a file.
FFTW_INCLUDES:PATH=/opt/local/include

//Path to a library.
FFTW_LIBRARY:FILEPATH=/opt/local/lib/libfftw3f.dylib

//Path to a library.
FFTW_THREADS_LIBRARY:FILEPATH=/opt/local/lib/libfftw3f_threads.dylib
```

in this case it was installed to /opt/local/lib, otherwise adapt the path 

8.3. set the correct OS X and path to the corresponding SDK

```
//The minimum version of OS X to support
CMAKE_OSX_DEPLOYMENT_TARGET:STRING=10.9

//The product will be built against the headers and libraries located
// inside the indicated SDK.
CMAKE_OSX_SYSROOT:STRING=/Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX10.9.sdk
```

8.4. OpenCL does not work properly on Macs. There is a bug in the implementation so we want to disable it to speed up the compilation process

```
//Build Drude implementation for OpenCL
OPENMM_BUILD_DRUDE_OPENCL_LIB:BOOL=OFF

//Build OpenMMOpenCL library
OPENMM_BUILD_OPENCL_LIB:BOOL=OFF

//Build RPMD implementation for OpenCL
OPENMM_BUILD_RPMD_OPENCL_LIB:BOOL=OFF
```

9. Run CMake again

```cmake .```

10. build openmm using (with ### of parallel processes)

```make [-j ###] install```

11. If everything was correct then OpenMM should finally install correctly

12. Optional we can create and run test files using 

```make test```

or run the single files with
```./Test[...]```