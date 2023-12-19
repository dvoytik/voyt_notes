How to build the Linux Kernel for RISC-V
----------------------------------------

Build-time depencies:
```
sudo pacman -S bc
```

Download the source code:
```
KERNEL_VER=v6.6.7
git clone --depth 1 --branch ${KERNEL_VER} https://github.com/torvalds/linux.git linux
cd linux
```

Build the kernel:
```
export CCPREFIX=riscv64-unknown-linux-gnu-
make ARCH=riscv CROSS_COMPILE=$CCPREFIX defconfig
make -j $(nproc) ARCH=riscv CROSS_COMPILE=$CCPREFIX
```

The uncompressed kernel imge will be located:
```
ls -l arch/riscv/boot/Image
-rwxr-xr-x 1 voyt voyt 22M Nov  5 23:39 arch/riscv/boot/Image*
```
