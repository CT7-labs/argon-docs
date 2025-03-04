# Instruction Format

Instructions are made of two 16-bit words. The first word is stored at the smaller address than the second word (big-Endian).

Argon is a load-store architecture processor. Before performing operations on data, values must be loaded from memory into registers.

### Instruction bits (first word)
`OOOO OOOI FFFF DDDD`

- 7-bit opcode
- 1-bit immediate flag (1 if last word is imm16, else 0)
- 4-bit flags
- 4-bit destination register index

### Instruction bits (second word)
`AAAA BBBB RRRR RRRR`

- 4-bit register A index
- 4-bit register B index
- 8 reserved bits

### OR
`IIII IIII IIII IIII`

- 16-bit immediate

## Instruction layout examples

Where no immediate is required:
`OOOOOOOI VVVVDDDD AAAABBBB RRRRRRRR`

Where a 16-bit immediate is required:
`OOOOOOOI VVVVDDDD IIIIIIII IIIIIIII`