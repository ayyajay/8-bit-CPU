# 8-bit CPU — Logisim Design

A custom **8-bit CPU** designed from scratch in [Logisim](http://www.cburch.com/logisim/), featuring a 16-bit instruction set, 8 general-purpose registers, an ALU with 7 operations, and subroutine support.

## Architecture Overview

```
┌──────────┐     ┌───────────────┐     ┌──────────────┐
│ Program  │────▶│  ROM (256×16) │────▶│ Instruction  │
│ Counter  │     │  (Program)    │     │  Register    │
└──────────┘     └───────────────┘     └──────────────┘
      ▲                                  │  │  │  │
      │            ┌──────────────┐      │  │  │  │
      └────────────│ Control Unit │◀─────┘  │  │  │
                   └──────────────┘         │  │  │
                     │  │  │  │             │  │  │
      ┌──────────────┘  │  │  │    OpCode──┘  │  │
      ▼                 ▼  │  │    Ra,Rb,Rd───┘  │
┌──────────┐    ┌────────┐ │  │    Immediate─────┘
│   RAM    │    │Register│ │  │
│(256 × 8) │    │  File  │ │  │
└──────────┘    │ R0–R7  │ │  │
      ▲         └────────┘ │  │
      │           │  │     │  │
      │           ▼  ▼     ▼  │
      │         ┌────────────┐│
      │         │    ALU     ││
      │         └────────────┘│
      │              │        │
      │              ▼        │
      │       ┌──────────────┐│
      └───────│Write Selector│◀
              └──────────────┘
```

## Key Specifications

| Feature | Value |
|---|---|
| Data Width | 8-bit |
| Instruction Width | 16-bit |
| Registers | 8 (R0–R7), R7 = Memory Address Register |
| Program Memory (ROM) | 256 × 16-bit |
| Data Memory (RAM) | 256 × 8-bit |
| ALU Operations | ADD, SUB, AND, NOR, INC, DEC, RSH |
| Condition Flags | Carry (C), Zero (Z), Negative (N) |
| Subroutine Support | CALL/RET with link register |
| Clock | Single-phase |

## Instruction Set (16 Opcodes)

### Instruction Format

```
Bit:  15 14 13 12 | 11 10  9 |  8  7  6 |  5  4  3 |  2  1  0
      ──────────── ────────── ────────── ────────── ──────────
       OpCode[3:0]  Rb[2:0]   F_Sel/imm   Ra[2:0]    Rd[2:0]
```

### Instructions

| OpCode | Mnemonic | Description |
|--------|----------|-------------|
| `0000` | `NOP` | No operation |
| `0001` | `HLT` | Halt execution |
| `0010` | `ADD` | Rd = Ra + Rb |
| `0011` | `SUB` | Rd = Ra − Rb |
| `0100` | `AND` | Rd = Ra AND Rb |
| `0101` | `NOR` | Rd = NOR(Ra, Rb) |
| `0110` | `INC` | Rd = Ra + 1 |
| `0111` | `DEC` | Rd = Ra − 1 |
| `1000` | `RSH` | Rd = Ra >> 1 (logical right shift) |
| `1001` | `LDI` | Rd = immediate value |
| `1010` | `LOD` | Rd = RAM[R7] (load from memory) |
| `1011` | `STR` | RAM[R7] = Ra (store to memory) |
| `1100` | `JMP` | PC = jump address |
| `1101` | `BIF` | Branch if flag condition met |
| `1110` | `CAL` | Call subroutine (saves return address) |
| `1111` | `RET` | Return from subroutine |

## Sub-Circuits

The CPU is composed of **7 modules**:

1. **Program Counter** — 8-bit PC with link register for CALL/RET
2. **Instruction Register (IR)** — Latches 16-bit instruction, splits into fields
3. **Register File** — 8 × 8-bit registers with dual-read, single-write
4. **ALU** — Arithmetic & logic unit with carry, zero, negative flags
5. **Control Unit** — Decodes opcode into control signals (combinational + HLT latch)
6. **Write Selector** — MUX to select write-back source (ALU / RAM / Immediate)
7. **Flag Register** — Stores C, Z, N flags

## How to Use

1. Download and install [Logisim](http://www.cburch.com/logisim/)
2. Open `CPU Architecture and Circuit.circ`
3. Use the **Poke Tool** to toggle the clock and step through execution
4. The ROM comes pre-loaded with a demo program

## Files

| File | Description |
|---|---|
| `CPU Architecture and Circuit.circ` | Main CPU design (Logisim) |
| `README.md` | Architecture and instruction set documentation |

## Tools Used

- [Logisim 2.7.1](http://www.cburch.com/logisim/) — Digital circuit simulator
