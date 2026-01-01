# [The C language](https://pyjamabrah.com/products/c-language/)


This repository is sand box environment for [the C Language Course by Pyjamabrah.](https://pyjamabrah.com/products/c-language/)

![](https://pyjamabrah.com/products/c-language/c-course.png)

This file documents the commands and documents used as part of the course.

---

### Toolchain

Execute the following commands to install the toolchain, QEMU and GDB

```bash
sudo apt update -y
sudo apt install -y gcc-riscv64-unknown-elf qemu-system-misc gdb-multiarch
```

### Quick makefile

```
# for compiling c-asm.c
claire: c-asm.c m.s m.ld
	riscv64-unknown-elf-gcc -O0 -ggdb -nostdlib -march=rv32i -mabi=ilp32 -Wl,-Tm.ld m.s c-asm.c -o main.elf
	riscv64-unknown-elf-objcopy -O binary main.elf main.bin

# old
main.bin: m.s m.ld
	riscv64-unknown-elf-gcc -O0 -ggdb -nostdlib -march=rv32i -mabi=ilp32 -Wl,-Tm.ld m.s -o main.elf
	riscv64-unknown-elf-objcopy -O binary main.elf main.bin

printbinary: main.bin
	xxd -e -c 4 -g 4 main.bin

startqemu: main.elf
	qemu-system-riscv32 -S -M virt -nographic -bios none -kernel main.elf -gdb tcp::1234
# kill qemu: ctrl + a (release) + x

connectgdb: main.elf
	gdb-multiarch main.elf -ex "target remote localhost:1234" -ex "break _start" -ex "continue" -q
# to quit gbd: `q`

assembly: c-asm.c
	riscv64-unknown-elf-gcc -O0 -ggdb -nostdlib -march=rv32i -mabi=ilp32 -Wl,-Tm. c-asm.c -S
    
clean:
	rm -rf *.out *.bin *.elf 
```

### RISC-V Reference Card

Details of the RISC-V 32i Instruction Encoding: [Download the PDF](https://github.com/jameslzhu/riscv-card/releases/download/latest/riscv-card.pdf)

### Tools
1. [RISC-V Instruction Decoder](https://luplab.gitlab.io/rvcodecjs/)
1. [GDB Dashboard](https://github.com/cyrus-and/gdb-dashboard)
1. [Assembler (as) Documentation](https://ftp.gnu.org/old-gnu/Manuals/gas/html_chapter/as_7.html)
    1. [Document for RISC-V Assembler](https://sourceware.org/binutils/docs-2.31/as/RISC_002dV_002dDirectives.html)

### C
1. [ISO Standard](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n1570.pdf)
1. [GNU C Manual](https://www.gnu.org/software/gnu-c-manual/gnu-c-manual.pdf)

# License and Support
```
All rights reserved. For more information contact support@pyjamabrah.com
```
