# gromacs 2022.4 gpu windows

A pre-built native version of GROMACS 2022.4 gpu for windows system useres.

Compiled by MSVC 17 2022 cmake with nVidia CUDA toolkit 11.8 & fftw 3.3.10

####################################

Compile method is quite simple 

Install VS 2022 community and open the 'x64 Native Tools Command Prompt for VS 2022.'

A. comile fftw 3.3.10

cmake . -DCMAKE_INSTALL_PREFIX=XXX -DENABLE_SSE2=ON -DENABLE_AVX=ON -DENABLE_AVX2=ON -DENABLE_AVX512=ON -DENABLE_FLOAT=ON -DBUILD_SHARED_LIBS=ON -G "Visual Studio 17 2022"

cmake --build . --target INSTALL --config Release

B. compile the gmx

tar xfz gromacs-2022.4.tar.gz

cd gromacs-2022.4

mkdir build

cd build

cmake .. -DCMAKE_INSTALL_PREFIX=XXX -DGMX_FFT_LIBRARY=fftw3 -DCMAKE_PREFIX_PATH=XXX -G "Visual Studio 17 2022" -DGMX_GPU=CUDA -DCUDA_TOOLKIT_ROOT_DIR=XXX -DCMAKE_BUILD_TYPE=Release

! open the gromacs.sln in VS 2022

! manually setting the environment to x64 release (active).

! changes all setting of C++ files from MD to MT.

cmake --build . --target INSTALL --config Release

#######################################

Several source code are modified to pass the check of MSVC compiler.

Including C++17 to CUDA17, error link 2038, some brackets, some header files, blablala ...

For test ONLY.

And welcome to report benchmark tests or bugs.
