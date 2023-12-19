ELF (Executable and Linkable Format)
------------------------------------

```
+-------------+
| ELF Header  |  readelf --file-header
|+---------+  |
||SEGMENT 0|  |  readelf --segments
||+-------+|  |
|||section||  |  readelf --sections
||+-------+|  |
||+-------+|  |
|||section||  |
||+-------+|  |
|+---------+  |
|+---------+  |
||SEGMENT 1|  |
||         |  |
||         |  |
|+---------+  |
|             |
+-------------+
```

Show all sections:
```
↪ readelf --wide --sections
```
```
There are 31 section headers, starting at offset 0x11fb448:

Section Headers:
  [Nr] Name              Type            Address          Off    Size   ES Flg Lk Inf Al
  [ 0]                   NULL            0000000000000000 000000 000000 00      0   0  0
  [ 1] .head.text        PROGBITS        ffffffff80000000 001000 001e9c 00  AX  0   0 4096
  [ 2] .text             PROGBITS        ffffffff80002000 003000 8b0c22 00  AX  0   0  8
  [ 3] .init.text        PROGBITS        ffffffff80a00000 a00000 04550e 00  AX  0   0 2097152
  [ 4] .exit.text        PROGBITS        ffffffff80a45510 a45510 002020 00  AX  0   0  2
  [ 5] .init.data        PROGBITS        ffffffff80c00000 a48000 0181a0 00  WA  0   0 4096
  [ 6] .init.pi          PROGBITS        ffffffff80c181a0 a601a0 002563 00 WAX  0   0  8
  [ 7] .init.bss         NOBITS          ffffffff80c1a708 a62703 000040 00  WA  0   0  8
  [ 8] .data..percpu     PROGBITS        ffffffff80c1b000 a63000 00a038 00  WA  0   0 64
  [ 9] .alternative      PROGBITS        ffffffff80c25038 a6d038 001100 00   A  0   0  1
  [10] .rodata           PROGBITS        ffffffff80e00000 a6f000 23cf40 00  WA  0   0 64
  [11] .pci_fixup        PROGBITS        ffffffff8103cf40 cabf40 003f78 00   A  0   0  8
  [12] __ksymtab         PROGBITS        ffffffff81040eb8 cafeb8 01b990 00   A  0   0  4
  [13] __ksymtab_gpl     PROGBITS        ffffffff8105c848 ccb848 01fc68 00   A  0   0  4
  [14] __ksymtab_strings PROGBITS        ffffffff8107c4b0 ceb4b0 02f7fb 01 AMS  0   0  1
  [15] __param           PROGBITS        ffffffff810abcb0 d1acb0 002e68 00   A  0   0  8
  [16] __modver          PROGBITS        ffffffff810aeb18 d1db18 000240 00  WA  0   0  8
  [17] __ex_table        PROGBITS        ffffffff810aed58 d1dd58 0019bc 00   A  0   0  4
  [18] .notes            NOTE            ffffffff810b0714 d1f714 000054 00   A  0   0  4
  [19] .srodata          PROGBITS        ffffffff81200000 d20000 000e68 00   A  0   0  8
  [20] .data             PROGBITS        ffffffff81400000 d21000 0e79e0 00  WA  0   0 4096
  [21] __bug_table       PROGBITS        ffffffff814e79e0 e089e0 018864 00  WA  0   0  1
  [22] .sdata            PROGBITS        ffffffff81500248 e21248 0015dc 00  WA  0   0  8
  [23] .got              PROGBITS        ffffffff81501828 e22828 000020 08  WA  0   0  8
  [24] .pecoff_edata_padding PROGBITS    ffffffff81501848 e22848 0001b8 00   A  0   0  1
  [25] .sbss             NOBITS          ffffffff81502000 e22a00 0027cd 00  WA  0   0 64
  [26] .bss              NOBITS          ffffffff81505000 e22a00 075fb0 00  WA  0   0 4096
  [27] .comment          PROGBITS        0000000000000000 e22a00 00001b 01  MS  0   0  1
  [28] .symtab           SYMTAB          0000000000000000 e22a20 222ae0 18     29 70932  8
  [29] .strtab           STRTAB          0000000000000000 1045500 1b5e26 00      0   0  1
  [30] .shstrtab         STRTAB          0000000000000000 11fb326 00011b 00      0   0  1
Key to Flags:
  W (write), A (alloc), X (execute), M (merge), S (strings), I (info),
  L (link order), O (extra OS processing required), G (group), T (TLS),
  C (compressed), x (unknown), o (OS specific), E (exclude),
  D (mbind), p (processor specific)
```

Show all segments and section to segment mapping:
```
↪ readelf --wide --segments vmlinux
```
```
Elf file type is EXEC (Executable file)
Entry point 0xffffffff80000000
There are 9 program headers, starting at offset 64

Program Headers:
  Type           Offset   VirtAddr           PhysAddr           FileSiz  MemSiz   Flg Align
  LOAD           0x001000 0xffffffff80000000 0x0000000000000000 0x8b2c22 0x8b2c22 R E 0x1000
  LOAD           0xa00000 0xffffffff80a00000 0x0000000000a00000 0x047530 0x047530 R E 0x200000
  LOAD           0xa48000 0xffffffff80c00000 0x0000000000c00000 0x01a703 0x01a748 RWE 0x1000
  LOAD           0xa63000 0xffffffff80c1b000 0x0000000000c1b000 0x00b138 0x00b138 RW  0x1000
  LOAD           0xa6f000 0xffffffff80e00000 0x0000000000e00000 0x2b0768 0x2b0768 RW  0x1000
  LOAD           0xd20000 0xffffffff81200000 0x0000000001200000 0x000e68 0x000e68 R   0x1000
  LOAD           0xd21000 0xffffffff81400000 0x0000000001400000 0x101a00 0x17afb0 RW  0x1000
  NOTE           0xd1f714 0xffffffff810b0714 0x00000000010b0714 0x000054 0x000054 R   0x4
  GNU_STACK      0x000000 0x0000000000000000 0x0000000000000000 0x000000 0x000000 RW  0x10

 Section to Segment mapping:
  Segment Sections...
   00     .head.text .text
   01     .init.text .exit.text
   02     .init.data .init.pi .init.bss
   03     .data..percpu .alternative
   04     .rodata .pci_fixup __ksymtab __ksymtab_gpl __ksymtab_strings __param __modver __ex_table .notes
   05     .srodata
   06     .data __bug_table .sdata .got .pecoff_edata_padding .sbss .bss
   07     .notes
   08
```
