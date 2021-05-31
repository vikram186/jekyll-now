---
layout: post
title: Compiling bcc and bpftrace from sources
category: bpf
description: 
author: <a href="https://arkivm.github.io/">Vikram Narayanan</a>
---

Notes on how to compile and install `bcc` and `bpftrace` from sources on Ubuntu 18.04 with a recent HWE enabled kernel `v5.4.0-73-generic`

* Enable BPF related kernel configs. The ubuntu kernel comes pre-built with BPF configs. Enable these [configs](https://github.com/iovisor/bcc/blob/master/INSTALL.md#kernel-configuration)
```
CONFIG_BPF=y
CONFIG_BPF_SYSCALL=y
# [optional, for tc filters]
CONFIG_NET_CLS_BPF=m
# [optional, for tc actions]
CONFIG_NET_ACT_BPF=m
CONFIG_BPF_JIT=y
# [for Linux kernel versions 4.1 through 4.6]
CONFIG_HAVE_BPF_JIT=y
# [for Linux kernel versions 4.7 and later]
CONFIG_HAVE_EBPF_JIT=y
# [optional, for kprobes]
CONFIG_BPF_EVENTS=y
# Need kernel headers through /sys/kernel/kheaders.tar.xz
CONFIG_IKHEADERS=y
```

* Install build deps
```
# For Bionic (18.04 LTS)
sudo apt-get -y install bison build-essential cmake flex git libedit-dev \
  python zlib1g-dev libelf-dev libfl-dev
```

* Install Googletest and googlemock
```
git clone https://github.com/google/googletest.git
mkdir build && cd build;
make -j
sudo make install
```

* Install llvm-12
```
wget https://apt.llvm.org/llvm.sh
chmod +x llvm.sh
sudo ./llvm.sh 12
```

* Update alternatives to point to v12
```
wget https://gist.githubusercontent.com/arkivm/e3f2dec38ef258d0fa1a23ebd4342a33/raw/92f0e73d2558402b7316021c1ab408b30e534de6/update-alternatives-clang.sh
./update-alternatives-clang.sh <version> <prio>
```

* Install `bcc`
	- Without a special commandline (`LLVM_SHARED`), you may likely encounter this [issue](https://github.com/iovisor/bpftrace/issues/251)
	- Use the cmake flags as described [here](https://github.com/iovisor/bpftrace/commit/debc79ef9ad4784258705a92ae70f9c7689a9c24)
```
git clone https://github.com/iovisor/bcc.git
mkdir bcc/build; cd bcc/build
cmake .. -DCMAKE_BUILD_TYPE=Debug -DCMAKE_INSTALL_PREFIX=... -DENABLE_TESTS=0 -DENABLE_MAN=0   -DENABLE_LLVM_SHARED=1
make -j
sudo make install
cmake -DPYTHON_CMD=python3 .. # build python3 binding
pushd src/python/
make
sudo make install
popd
```

* Install `bpftrace`
```
sudo apt-get install -y bison cmake flex g++ git libelf-dev zlib1g-dev libfl-dev systemtap-sdt-dev binutils-dev
git clone https://github.com/iovisor/bpftrace
mkdir bpftrace/build && cd bpftrace/build;
cmake -DCMAKE_BUILD_TYPE=Release ..
make -j
sudo make install
```

