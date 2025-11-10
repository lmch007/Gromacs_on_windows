# Gromacs 2024.6 GPU Windows
[⬇️ Download (Latest Release)](https://github.com/iriscite/Gromacs_on_windows/releases/latest)


**A pre-built native version of GROMACS 2024.6 GPU for Windows system users.**

**NVIDIA 50-series GPUs are not supported (CUDA needs ≥12.8).**

Built with CMake (generator: Visual Studio 17 2022, MSVC toolset), NVIDIA CUDA Toolkit 11.8, and FFTW 3.3.10.

---

Install VS 2022 Community and open the '**x64 Native Tools Command Prompt for VS 2022**.'

*A. Compile fftw 3.3.10*
```
cmake . -DCMAKE_INSTALL_PREFIX=XXX -DENABLE_SSE2=ON -DENABLE_AVX=ON -DENABLE_AVX2=ON -DENABLE_AVX512=ON -DENABLE_FLOAT=ON -DBUILD_SHARED_LIBS=ON -G "Visual Studio 17 2022"

cmake --build . --target INSTALL --config Release
```
*B. Modify the source code*

### 1.&emsp;  ./cmake/gmxManageNvccConfig.cmake

```diff
-list(APPEND GMX_CUDA_NVCC_FLAGS "${CMAKE_CXX17_STANDARD_COMPILE_OPTION}")
+list(APPEND GMX_CUDA_NVCC_FLAGS "${CMAKE_CUDA17_STANDARD_COMPILE_OPTION}")
```
Then
```diff
-gmx_add_nvcc_flag_if_supported(GMX_CUDA_NVCC_FLAGS NVCC_HAS_USE_FAST_MATH -use_fast_math)
+gmx_add_nvcc_flag_if_supported(GMX_CUDA_NVCC_FLAGS NVCC_HAS_USE_FAST_MATH -use_fast_math -std=c++17)
```

### 2.&emsp;  ./CMakeLists.txt

In the first line (you can below the annotation)

```diff
+set(CMAKE_POLICY_DEFAULT_CMP0091 NEW)
+set(CMAKE_POLICY_DEFAULT_CMP NEW)
+cmake_policy(SET CMP0091 NEW)
```
Then, under 
```cmake
project(Gromacs VERSION 2024.6)
```
Add
```diff
+if(MSVC)
+    set(CMAKE_MSVC_RUNTIME_LIBRARY "MultiThreaded$<$<CONFIG:Debug>:Debug>")
+    string(REPLACE "/MD" "/MT" CMAKE_CXX_FLAGS_RELEASE "${CMAKE_CXX_FLAGS_RELEASE}")
+    string(REPLACE "/MDd" "/MTd" CMAKE_CXX_FLAGS_DEBUG "${CMAKE_CXX_FLAGS_DEBUG}")
+endif()
```

*C. Compile gmx 2024.6*

The compilation method is quite simple: 

```
tar -xvzf gromacs-2024.6.tar.gz

cd gromacs-2024.6 && mkdir build && cd build

cmake .. -DCMAKE_INSTALL_PREFIX="XXX" -DGMX_FFT_LIBRARY=fftw3 -DCMAKE_PREFIX_PATH="XXX" -G "Visual Studio 17 2022" -DGMX_SIMD=AVX2_256 -DGMX_GPU=CUDA -DCUDA_TOOLKIT_ROOT_DIR="XXX" -DCMAKE_BUILD_TYPE=Release

cmake --build . --target INSTALL --config Release
```

All done. Enjoy it.

#######################################

**WARNING: To pass the MSVC compiler check, _some code is changed_.**

**Please use with caution.**

And feel free to report benchmark results or bugs.

**Reference:** [keinsci 37179](http://bbs.keinsci.com/thread-37179-1-1.html)


