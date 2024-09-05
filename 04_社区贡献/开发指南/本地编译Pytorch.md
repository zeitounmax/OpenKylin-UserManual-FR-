### 本地编译Pytorch
### **获取源代码**
链接：https://pan.baidu.com/s/1-kaWPzqAb6g7l4Bdhh-KPw?pwd=843d 

提取码：843d 

源码包为原始pytorch仓库的完整克隆，克隆日期为2月20日，需要最新版本的请自行拉取
### **修改源代码**
##### **荔枝派和算能开发板**
对于荔枝派和算能的硬件环境，需要将源代码目录中的  pytorch/third_party/cpuinfo目录替换为算能的cpuinfo目录，具体如下
```
cd pytorch/third_party
# 删除原有cpuinfo目录
sudo rm -r cpuinfo 
# 克隆算能的cpuinfo目录
sudo git clone https://github.com/sophgo/cpuinfo.git

```
##### **其他硬件环境**
对于其他硬件环境，可以暂时不替换cpuinfo目录，建议替换
##### **修改代码文件**
**1.aten/src/ATen/CMakeLists.txt**

将语句：
```
if(NOT MSVC AND NOT EMSCRIPTEN AND NOT INTERN_BUILD_MOBILE)

```
替换为：
```
if(FALSE)
```
**2.caffe2/CMakeLists.txt**

将语句：
```
target_link_libraries(${test_name}_${CPU_CAPABILITY} c10 sleef gtest_main)
```

替换为：
```
target_link_libraries(${test_name}_${CPU_CAPABILITY} c10 gtest_main)
```
**3.test/cpp/api/CMakeLists.txt**

在语句：
```
add_executable(test_api ${TORCH_API_TEST_SOURCES})
```
后添加：
```
target_compile_options(test_api PUBLIC -Wno-nonnull)
```
### **编译工具链安装**
```
apt install libopenblas-dev libblas-dev m4 cmake cython3 ccache

# 若执行过程中有以下报错，则杀死10198进程:Waiting for cache lock: Could not get lock /var/lib/dpkg/lock-frontend. It is held by process 10198 (apt)
kill -9 10198

# 安装过程中，若发现libopenblas-dev无法正常安装，则跳过其直接安装libblas-dev m4 cmake cython3 ccache
apt install libblas-dev m4 cmake cython3 ccache

# libopenblas-dev无法安装的替代方案：手动安装OpenBLAS
git clone https://github.com/xianyi/OpenBLAS.git
cd OpenBLAS
make -j8
sudo make PREFIX=/usr/local/OpenBLAS install
# 进入/etc，用vim打开profile
[在最后一行添加]: export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/OpenBLAS/lib/
source /etc/profile

```
### Python环境配置
```
apt install python3
apt install python-is-python3 python-dev-is-python3
apt autoremove
# 下载好python3后发现没有pip、yaml等工具
apt install python3-pip
pip install pyyaml

```
##### 编译前安装Numpy
```
python3 -m pip install numpy --upgrade -i    
https://pypi.tuna.tsinghua.edu.cn/simple
```
### **环境变量配置**
```
export USE_CUDA=0 #不编译CUDA版本
export USE_DISTRIBUTED=0 #不支持分布式
export USE_MKLDNN=0 #不支持MKLDNN
export MAX_J0BS=16 #最大线程数 ，按需填写
```
### **编译pytorch前注意事项**

● 一定要确认完整克隆好，如果对自己的网络没有信心，请直接从百度网盘下载源码包，注意源码包为原始pytorch仓库的完整克隆，克隆日期为2月20日，需要最新版本的请自行拉取

● 确认gcc版本

```
gcc -v
```

gcc13以下的版本会遇到undefined reference to `__atomic_exchange_1'问题，可以通过patchelf解决，或者通过patch CMakefile文件的方式解决。
```
# path为存放libtorch_cpu.so的路径
patchelf --add-needed libatomic.so.1 /path/libtorch_cpu.so
# 若提示无patchelf命令，则执行下列语句
apt install patchelf
```

patch内容详情：
```
From 40bae41e910c2aa0cc4d79457199152a21b99add Mon Sep 17 00:00:00 2001
From: Bits <bigbits@hackx.cc>
Date: Thu, 22 Feb 2024 10:09:15 +0800
Subject: [PATCH] =?UTF-8?q?GCC13=E4=BB=A5=E4=B8=8B=E7=89=88=E6=9C=AC?=
 =?UTF-8?q?=E7=BC=96=E8=AF=91=E9=9C=80=E8=A6=81=E9=93=BE=E6=8E=A5atomic?=
 =?UTF-8?q?=E5=BA=93?=
MIME-Version: 1.0
Content-Type: text/plain; charset=UTF-8
Content-Transfer-Encoding: 8bit

---
 caffe2/CMakeLists.txt              | 6 +++---
 test/cpp/api/CMakeLists.txt        | 4 ++--
 test/cpp/jit/CMakeLists.txt        | 2 +-
 test/cpp/lazy/CMakeLists.txt       | 2 +-
 test/cpp/tensorexpr/CMakeLists.txt | 4 ++--
 test/edge/CMakeLists.txt           | 2 +-
 torch/lib/libshm/CMakeLists.txt    | 2 +-
 7 files changed, 11 insertions(+), 11 deletions(-)

diff --git a/caffe2/CMakeLists.txt b/caffe2/CMakeLists.txt
index 17582104933..ebfd4c1c10e 100644
--- a/caffe2/CMakeLists.txt
+++ b/caffe2/CMakeLists.txt
@@ -1425,7 +1425,7 @@ if($ENV{TH_BINARY_BUILD})
   endif()
 endif()
 
-target_link_libraries(torch_cpu PUBLIC c10)
+target_link_libraries(torch_cpu PUBLIC c10 atomic)
 target_link_libraries(torch_cpu PUBLIC ${Caffe2_PUBLIC_DEPENDENCY_LIBS})
 target_link_libraries(torch_cpu PRIVATE ${Caffe2_DEPENDENCY_LIBS})
 target_link_libraries(torch_cpu PRIVATE ${Caffe2_DEPENDENCY_WHOLE_LINK_LIBS})
@@ -1750,7 +1750,7 @@ if(BUILD_TEST)
         if(NOT MSVC)
           add_executable(${test_name}_${CPU_CAPABILITY} "${test_src}" ../aten/src/ATen/native/quantized/AffineQuantizerBase.cpp)
           # TODO: Get rid of c10 dependency (which is only needed for the implementation of AT_ERROR)
-          target_link_libraries(${test_name}_${CPU_CAPABILITY} c10 sleef gtest_main)
+          target_link_libraries(${test_name}_${CPU_CAPABILITY} c10 sleef gtest_main atomic)
           if(USE_FBGEMM)
             target_link_libraries(${test_name}_${CPU_CAPABILITY} fbgemm)
           endif()
@@ -1781,7 +1781,7 @@ if(BUILD_TEST)
   foreach(test_src ${Caffe2_CPU_TEST_SRCS})
     get_filename_component(test_name ${test_src} NAME_WE)
     add_executable(${test_name} "${test_src}")
-    target_link_libraries(${test_name} torch_library gtest_main)
+    target_link_libraries(${test_name} torch_library gtest_main atomic)
     target_include_directories(${test_name} PRIVATE $<INSTALL_INTERFACE:include>)
     target_include_directories(${test_name} PRIVATE $<BUILD_INTERFACE:${CMAKE_BINARY_DIR}/include>)
     target_include_directories(${test_name} PRIVATE ${Caffe2_CPU_INCLUDE})
diff --git a/test/cpp/api/CMakeLists.txt b/test/cpp/api/CMakeLists.txt
index 42b67d8cb25..3be0b803828 100644
--- a/test/cpp/api/CMakeLists.txt
+++ b/test/cpp/api/CMakeLists.txt
@@ -49,7 +49,7 @@ endif()
 
 add_executable(test_api ${TORCH_API_TEST_SOURCES})
 target_include_directories(test_api PRIVATE ${ATen_CPU_INCLUDE})
-target_link_libraries(test_api PRIVATE torch gtest)
+target_link_libraries(test_api PRIVATE torch gtest atomic)
 if(NOT MSVC)
   target_compile_options_if_supported(test_api -Wno-unused-variable)
 endif()
@@ -79,4 +79,4 @@ endif()
 
 add_executable(parallel_benchmark ${TORCH_API_TEST_DIR}/parallel_benchmark.cpp)
 target_include_directories(parallel_benchmark PRIVATE ${ATen_CPU_INCLUDE})
-target_link_libraries(parallel_benchmark PRIVATE torch)
+target_link_libraries(parallel_benchmark PRIVATE torch atomic)
diff --git a/test/cpp/jit/CMakeLists.txt b/test/cpp/jit/CMakeLists.txt
index 2d88d3f7172..17a268b89a0 100644
--- a/test/cpp/jit/CMakeLists.txt
+++ b/test/cpp/jit/CMakeLists.txt
@@ -107,7 +107,7 @@ if(USE_ASAN)
 endif()
 
 target_link_libraries(
-  test_jit PRIVATE flatbuffers)
+  test_jit PRIVATE flatbuffers atomic)
 
 
 # TODO temporary until we can delete the old gtest polyfills.
diff --git a/test/cpp/lazy/CMakeLists.txt b/test/cpp/lazy/CMakeLists.txt
index be37b47ac9b..aef985e6ea7 100644
--- a/test/cpp/lazy/CMakeLists.txt
+++ b/test/cpp/lazy/CMakeLists.txt
@@ -27,7 +27,7 @@ add_executable(test_lazy
 # TODO temporary until we can delete the old gtest polyfills.
 target_compile_definitions(test_lazy PRIVATE USE_GTEST)
 
-set(LAZY_TEST_DEPENDENCIES torch gtest)
+set(LAZY_TEST_DEPENDENCIES torch gtest atomic)
 
 target_link_libraries(test_lazy PRIVATE ${LAZY_TEST_DEPENDENCIES})
 target_include_directories(test_lazy PRIVATE ${ATen_CPU_INCLUDE})
diff --git a/test/cpp/tensorexpr/CMakeLists.txt b/test/cpp/tensorexpr/CMakeLists.txt
index 012471d0e58..5b0a4c540f3 100644
--- a/test/cpp/tensorexpr/CMakeLists.txt
+++ b/test/cpp/tensorexpr/CMakeLists.txt
@@ -39,7 +39,7 @@ add_executable(test_tensorexpr
   ${TENSOREXPR_TEST_ROOT}/padded_buffer.cpp
   ${TENSOREXPR_TEST_SRCS})
 
-target_link_libraries(test_tensorexpr PRIVATE torch gtest)
+target_link_libraries(test_tensorexpr PRIVATE torch gtest atomic)
 target_include_directories(test_tensorexpr PRIVATE ${ATen_CPU_INCLUDE})
 target_compile_definitions(test_tensorexpr PRIVATE USE_GTEST)
 if(NOT MSVC)
@@ -47,7 +47,7 @@ if(NOT MSVC)
 endif()
 
 add_executable(tutorial_tensorexpr ${TENSOREXPR_TEST_ROOT}/tutorial.cpp)
-target_link_libraries(tutorial_tensorexpr PRIVATE torch)
+target_link_libraries(tutorial_tensorexpr PRIVATE torch atomic)
 target_include_directories(tutorial_tensorexpr PRIVATE ${ATen_CPU_INCLUDE})
 
 # The test case depends on the xnnpack header which in turn depends on the
diff --git a/test/edge/CMakeLists.txt b/test/edge/CMakeLists.txt
index 2f29f27e0b3..311062891cf 100644
--- a/test/edge/CMakeLists.txt
+++ b/test/edge/CMakeLists.txt
@@ -58,7 +58,7 @@ add_executable(test_edge_op_registration
 
 target_compile_definitions(test_edge_op_registration PRIVATE USE_GTEST)
 
-set(TEST_DEPENDENCIES gtest unbox_lib)
+set(TEST_DEPENDENCIES gtest unbox_lib atomic)
 
 target_link_libraries(test_edge_op_registration PRIVATE
   ${TEST_DEPENDENCIES}
diff --git a/torch/lib/libshm/CMakeLists.txt b/torch/lib/libshm/CMakeLists.txt
index a3b41d0a0b0..a1db648342c 100644
--- a/torch/lib/libshm/CMakeLists.txt
+++ b/torch/lib/libshm/CMakeLists.txt
@@ -60,7 +60,7 @@ if(UNIX AND NOT APPLE)
 endif()
 
 add_executable(torch_shm_manager manager.cpp)
-target_link_libraries(torch_shm_manager PRIVATE shm c10)
+target_link_libraries(torch_shm_manager PRIVATE shm c10 atomic)
 set_target_properties(torch_shm_manager PROPERTIES
   INSTALL_RPATH "${_rpath_portable_origin}/../lib")
 
-- 
2.34.1



```
gcc13版本及以上没有此问题，但是可能出现软件包依赖问题，通过
```
export BUILD_TEST=0
```
可以解决。

● 确认安装ninja，如果系统不能执行ninja，请apt安装ninja-build包或者从源码编译安装。

● 编译前安装numpy

```
python3 -m pip install numpy --upgrade -i https://pypi.tuna.tsinghua.edu.cn/simple

```
● 构建指令可以用`python setup.py bdist_wheel` 替代 `python setup.py develop --cmake`可以省去sudo管理员权限，构建完成后将会在dist目录下生成whl安装包。

● 编译开始时显示的Failed不影响编译结果。缺少apt_pkg不影响编译。

###  **开始编译**
```
sudo python3 setup.py develop --cmake

```
###  **编译及引用过程可能会遇到的问题**
**1.Could not find any of CMakeLists.txt, Makefile, setup.py, LICENSE, LICENSE.md, LICENSE.txt in /root/xxx/pytorch/third_party/pthreadpool**

问题来源：编译过程

问题分析：由于网络的问题，clone仓库时有部分包未成功下载，导致文件夹为空

解决方法：重新下载对应包
```
cd third_party
rm -rf pthreadpool
sudo git clone pthreadpool的url
```
**2./usr/bin/ld: /root/xxx/pytorch/build/lib/libtorch_cpu.so: undefined reference to `__atomic_exchange_1’   collect2: error: ld returned 1 exit status**

 问题来源：编译过程

 问题分析：对__atomic_exchange_1的未定义引用

解决方法：使用patchelf添加需要的动态库

```
# path为存放libtorch_cpu.so的路径
patchelf --add-needed libatomic.so.1 /path/libtorch_cpu.so
# 若提示无patchelf命令，则执行下列语句
apt install patchelf
```
**3.Error in cpuinfo: processor architecture is not supported in cpuinfo**

问题来源：编译完成后，使用python时“import torch”报错

问题分析：git clone时下载的cpuinfo不支持Risc-V架构

解决方法：删除当前存在的cpuinfo并重新下载最新支持Risc-V架构的cpuinfo
cpuinfo路径为pytorch/third_party/cpuinfo，因此先进入pytorch/third_party目录
 在终端执行以下指令
 
```
rm -rf cpuinfo
git clone https://github.com/sophgo/cpuinfo.git
# clone完成后需要重新编译
sudo python3 setup.py develop --cmake

```
### 原文链接：[https://blog.csdn.net/m0_49267873/article/details/135670989](https://blog.csdn.net/m0_49267873/article/details/135670989)