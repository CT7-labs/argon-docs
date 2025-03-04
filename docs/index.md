# Argon

Argon is a fully custom 16-bit RISC CPU developed by myself. It's my second complex Verilog project.

### A little about me...
- I have zero formal training
- I implemented a custom breadboard computer when I was 16
- I implemented a custom graphics chip using Verilog (without tutorials)
- I've designed 3 custom PCBs with JLCPCB and PCBWay

### A little about Argon
- 16-bit data width
- 24-bit address bus (with banking)
- No pipelining (that's a little above my paygrade right now)
- 7-bit opcode with 3-bit flags
- 32-bit fixed size instructions
- Only 4 addressing modes
- Little-endian

## Address space
Argon can only address up to 64KW of memory at once, but has a 24-bit address thanks to the memory bank (MB) register.

The MB register holds two 8-bit pointers that control the base of each 32KW window that make up the immediately addressable 64KW of memory. The windows are separate from each other, and can even show the same 32KW space at once.

The lower window is visible from `0x0000-0x7FFF` and the upper window is visible from `0x8000-0xFFFF`. The base (in increments of 32KW) is controlled by the LSB and MSB in the MB register, respectively.

So if you wanted to see addresses `0x000000-0x007FFF` and `0x010000-0x017FFF` at once, you would load `0x0100` into the MB register.

If you wanted to see addresses `0xFF0000-0xFFFFFF` and `0x800000-0x807FFFF` at once, you would load `0xFF80` into the MB register.

## Register file
#### r0
Hardwired 0 register

#### r1-r5
General purpose registers

#### r6 (sp)
Stack pointer

Decrements on PUSH, increments on POP

#### r7 (mb)
Memory bank

`UUUUUUUU LLLLLLLL`

Upper byte controls the upper 32KW window into the 16MW address space.

Lower byte controls the lower 32KW window into the 16MW address space.

Both windows can cover the same 32KW.

## Memory-mapped registers
### Clock divider
Divides the CPU clock by stored value

### Interrupt control
- Global interrupt enable
- Interrupt mask 1-7
- 8 reserved bits

`EMMMMMMM RRRRRRRR`

Interrupt vectors are stored in memory, not registers