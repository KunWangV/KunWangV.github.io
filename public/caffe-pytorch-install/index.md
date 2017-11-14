---
title: caffe pytorch install
date: 2017-11-14 16:24:42
tags: caffe, DL,
---

> **简介：** 之前Python的虚拟环境一直使用的是 virtualenvwrapper, 用起来还挺好用的。但是呢，好像virtualenvwrapper对编译程序不太友好。
最近有需要编译PyTorch master版本，官方推荐使用anaconda 进行编译，貌似提供的mkl编一起来比较高效，为了避免踩坑，聪明的我采用了官方推荐的方式。
最新版的pytorch需要cudnn6.0+，于是又把cudnn升级了。pytorch安装完毕，caffe需要重新编译，因此有踩了一系列的坑，再此记录一哈。


# Pytorch 源码编译
## 官网步骤（[点这里](!https://github.com/pytorch/pytorch#from-source))
```
export CMAKE_PREFIX_PATH="$(dirname $(which conda))/../" # [anaconda root directory]

# Install basic dependencies
conda install numpy pyyaml mkl setuptools cmake cffi

# Add LAPACK support for the GPU
conda install -c soumith magma-cuda80 # or magma-cuda75 if CUDA 7.5

git clone --recursive https://github.com/pytorch/pytorch

python setup.py install

```

步骤很简单，不多说

## 遇到的问题

```
../libATen.so.1: undefined reference to `std::runtime_error::runtime_error(char const*)'
collect2: error: ld returned 1 exit status
src/ATen/test/CMakeFiles/scalar_tensor_test.dir/build.make:117: recipe for target 'src/ATen/test/scalar_tensor_test' failed
make[2]: *** [src/ATen/test/scalar_tensor_test] Error 1
CMakeFiles/Makefile2:375: recipe for target 'src/ATen/test/CMakeFiles/scalar_tensor_test.dir/all' failed
make[1]: *** [src/ATen/test/CMakeFiles/scalar_tensor_test.dir/all] Error 2
../libATen.so.1: undefined reference to `std::runtime_error::runtime_error(char const*)'
collect2: error: ld returned 1 exit status
src/ATen/test/CMakeFiles/basic.dir/build.make:117: recipe for target 'src/ATen/test/basic' failed
make[2]: *** [src/ATen/test/basic] Error 1
CMakeFiles/Makefile2:338: recipe for target 'src/ATen/test/CMakeFiles/basic.dir/all' failed
make[1]: *** [src/ATen/test/CMakeFiles/basic.dir/all] Error 2
../libATen.so.1: undefined reference to `std::runtime_error::runtime_error(char const*)'
collect2: error: ld returned 1 exit status
src/ATen/test/CMakeFiles/scalar_test.dir/build.make:117: recipe for target 'src/ATen/test/scalar_test' failed
make[2]: *** [src/ATen/test/scalar_test] Error 1
CMakeFiles/Makefile2:264: recipe for target 'src/ATen/test/CMakeFiles/scalar_test.dir/all' failed
make[1]: *** [src/ATen/test/CMakeFiles/scalar_test.dir/all] Error 2
Makefile:127: recipe for target 'all' failed
make: *** [all] Error 2
```

在pytorch forum问了下，说是gcc 版本的问题，反正最后编译通过


# Anaconda 编译Caffe接口 

> 安装pytorch步骤简单，错误也挺好结果，不得不说forum的回复很活跃很及时，方便了不少事情。点赞，打call。花了我最多时间的是caffe。


因为cudnn升级了caffe需要重新编译，但是问题来了，报google::protobuf::reference error。楼主也是一时脑抽，我的解决方法是：

- 1、gcc，g++版本
- 2、更改系统的protobuf版本

解决了一个上午也无济于事，使用whereis protoc 发现有两个protoc，一个是系统的，一个是anaconda自己的，因为我在anaconda上安装了keras，keras依赖Tensorflow
最新版的Tensorflow需要protobuf>=3.0，所以anaconda上面的protobuf是3.4.0版本的，而系统Ubuntu 16.04 安装的2.6.1版本的。所以产生了问题。那么解决方法是不
是可以升级系统的protobuf呢？楼主的方法：

**全面升级到3.4.0** 因为这样不需要卸载Tensorflow，需要改的只是系统的版本，结果：**行不通**，估计是caffe代码的问题


最终解决方法：

降级到protobuf 2.6.1，这时候遇到另外一个问题，opencv reference error，所以，问题类型一致，使用anaconda安装opencv3，卸载系统的opencv2，顺利解决，最终！
