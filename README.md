# Gromacs 2022.4 GPU Windows

**A pre-built native version of GROMACS 2022.4 GPU for Windows system users.**

Compiled by MSVC 17 2022 cmake with nVidia CUDA toolkit 11.8 & fftw 3.3.10

####################################

 The compilation method is quite simple: 

Install VS 2022 Community and open the '**x64 Native Tools Command Prompt for VS 2022**.'

*A. Compile fftw 3.3.10*
```
cmake . -DCMAKE_INSTALL_PREFIX=XXX -DENABLE_SSE2=ON -DENABLE_AVX=ON -DENABLE_AVX2=ON -DENABLE_AVX512=ON -DENABLE_FLOAT=ON -DBUILD_SHARED_LIBS=ON -G "Visual Studio 17 2022"

cmake --build . --target INSTALL --config Release
```
*B. Compile gmx 2022.4*
```
tar xfz gromacs-2022.4.tar.gz

cd gromacs-2022.4

mkdir build

cd build

cmake .. -DCMAKE_INSTALL_PREFIX=XXX -DGMX_FFT_LIBRARY=fftw3 -DCMAKE_PREFIX_PATH=XXX -G "Visual Studio 17 2022" -DGMX_GPU=CUDA -DCUDA_TOOLKIT_ROOT_DIR=XXX -DCMAKE_BUILD_TYPE=Release
```

*C. Open the gromacs.sln in VS 2022*

manually set the environment to x64 release (active).

change all setting of C++ files settings from MD to MT.
```
cmake --build . --target INSTALL --config Release
```
or right click INSTALL and click generate.

All done. Enjoy it.

#######################################

**WARNING: To pass the MSVC compiler check, _some code is changed_.**

Including C++17 to CUDA17, error link 2038, some brackets, some header files, blablala ...

**Please use with caution.**

And feel free to report benchmark tests or bugs.
