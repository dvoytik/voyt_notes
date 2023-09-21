https://riscv.org/wp-content/uploads/2019/12/Summit_bootflow.pdf

https://github.com/riscv-software-src/opensbi/blob/master/docs/platform/qemu_virt.md

https://github.com/riscv-software-src/opensbi/blob/master/docs/firmware/fw_payload.md



```
cd ~/p
git clone https://github.com/riscv-software-src/opensbi
export CROSS_COMPILE=riscv64-unknown-linux-gnu-
make PLATFORM=generic FW_PAYLOAD_PATH=$HOME/p/linux/arch/riscv/boot/Image
ls -l build/platform/generic/firmware/fw_payload.bin
```
