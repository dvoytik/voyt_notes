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

Busybox root FS
---------------
**TODO**: use build root. 
**TODO**: combine initramfs with kernel in one image

OpenSBI
-------

How to build OpenSBI firmware with Linux Kernel as Payload (FW_PAYLOAD):
```
cd ~/p
git clone https://github.com/riscv-software-src/opensbi
export CROSS_COMPILE=riscv64-unknown-linux-gnu-
make PLATFORM=generic FW_PAYLOAD_PATH=$HOME/p/linux/arch/riscv/boot/Image
ls -l build/platform/generic/firmware/fw_payload.bin
```

[references](riscv_boot.md)

For reference - Kernel image header
-----------------------------------

[Image header format](https://www.kernel.org/doc/Documentation/riscv/boot-image-header.rst):
```c
	u32 code0;		      /* Executable code */
	u32 code1;		      /* Executable code */
	u64 text_offset;	  /* Image load offset, little endian */
	u64 image_size;		  /* Effective Image size, little endian */
	u64 flags;		      /* kernel flags, little endian */
	u32 version;		  /* Version of this header */
	u32 res1 = 0;		  /* Reserved */
	u64 res2 = 0;		  /* Reserved */
	u64 magic = 0x5643534952; /* Magic number, little endian, "RISCV" */
	u32 magic2 = 0x05435352;  /* Magic number 2, little endian, "RSC\x05" */
	u32 res3;		          /* Reserved for PE COFF offset */
```

```
↪ hexdump -C -n 128 arch/riscv/boot/Image
00000000  4d 5a 6f 10 e0 0c 01 00  00 00 20 00 00 00 00 00  |MZo....... .....|
00000010  00 b0 57 01 00 00 00 00  00 00 00 00 00 00 00 00  |..W.............|
00000020  02 00 00 00 00 00 00 00  00 00 00 00 00 00 00 00  |................|
00000030  52 49 53 43 56 00 00 00  52 53 43 05 40 00 00 00  |RISCV...RSC.@...|
00000040  50 45 00 00 64 50 02 00  00 00 00 00 00 00 00 00  |PE..dP..........|
00000050  00 00 00 00 a0 00 06 02  0b 02 02 14 00 f0 bf 00  |................|
00000060  00 b0 97 00 00 00 00 00  fc b2 a3 00 00 10 00 00  |................|
00000070  00 00 00 00 00 00 00 00  00 10 00 00 00 02 00 00  |................|
00000080
```
First two bytes are replaced with `MZ` (0x4d 0x5a) magic string to support EFI
[Portable Executable](https://en.wikipedia.org/wiki/Portable_Executable) stub.

```
text_offset = 00 00 20 00 00 00 00 00 = 0x0020_0000 =  2 097 152 Bytes =  2 048 KiB
image_size   = 00 b0 57 01 00 00 00 00 = 0x0154_b000 = 22 327 296 Bytes = 21 804 KiB
```
