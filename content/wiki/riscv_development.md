---
title: RISC-V development
date: 2024-09-09
tags: ['computing']
toc: true
draft: false
---

This will build the RISC-V toolchain under `/opt/riscv` which requires sudo. You can use a different directory. The RISC-V GCC toolchain can also be installed prebuilt from most linux package managers.

## Installation
### RISC-V GCC

```
sudo apt-get install autoconf automake autotools-dev curl python3 libmpc-dev libmpfr-dev libgmp-dev gawk build-essential bison flex texinfo gperf libtool patchutils bc zlib1g-dev libexpat-dev
git clone https://github.com/riscv-collab/riscv-gnu-toolchain.git
cd riscv-gnu-toolchain
./configure --prefix=/opt/riscv --with-arch=rv64gc --with-abi=lp64d
sudo make
```

then add `/opt/riscv/bin` to `$PATH` and restart (pk requires a RISC-V compiler)

### pk
```
cd ..
git clone https://github.com/riscv-software-src/riscv-pk.git
cd riscv-pk
mkdir build
cd build
../configure --prefix=/opt/riscv --host=riscv64-unknown-elf
make
sudo make install
```

### Spike
```
cd ..
sudo apt-get install device-tree-compiler
git clone https://github.com/riscv-software-src/riscv-isa-sim
cd riscv-isa-sim
mkdir build
cd build
../configure --prefix=/opt/riscv
make
sudo make install
```

## Usage
Create a sample `test.c` file.
Then:
```
riscv64-unknown-elf-gcc test.c
spike --isa=RV64GC pk a.out
```
This will cross-compile the file using for RISC-V, specifically the `RV64GC` extension (or whatever extension passed in the GCC flags), and simulate it in the `Spike` RISC-V emulator. `pk` is a proxy kernel that intercepts syscalls in what's otherwise bare-metal code.