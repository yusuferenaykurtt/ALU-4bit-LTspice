# 4-bit Custom ALU — LTspice VLSI Design Project

> **Course:** BIMU4044 – VLSI Design  
> **Institution:** İstanbul Üniversitesi Cerrahpaşa, Bilgisayar Mühendisliği  
> **Date:** May 2026  

**Authors:**
- İsmail Çolak — 1306210024  
- Yusuf Eren Aykurt — 1306210062  

**Instructor:** Nihal Altuntaş

---

## Overview

This project implements a **4-bit custom Arithmetic Logic Unit (ALU)** entirely at the transistor level using LTspice. No SPICE digital primitives are used — every gate, combinational element, and sequential element is built from PMOS and NMOS transistors.

The ALU supports **8 operations** selected by a 3-bit opcode (S2, S1, S0), uses two 4-bit input registers (A and B), one 4-bit output register (C), and produces four status flags.

---

## Supported Operations

| Op # | S2 S1 S0 | Operation | Description |
|------|----------|-----------|-------------|
| 1 | 0 0 0 | NOT A | Bitwise complement of A (B ignored) |
| 2 | 0 0 1 | NAND(A, B) | Bitwise NAND on all bits of A and B |
| 3 | 0 1 0 | NOR(A, B) | Bitwise NOR on all bits of A and B |
| 4 | 0 1 1 | XOR(A, B) | Bitwise XOR on all bits of A and B |
| 5 | 1 0 0 | XNOR(A, B) | Bitwise XNOR on all bits of A and B |
| 6 | 1 0 1 | A + B | Arithmetic addition |
| 7 | 1 1 0 | A - B | Arithmetic subtraction (two's complement) |
| 8 | 1 1 1 | A << 1 | Left shift by 1 bit, LSB filled with 0 |

---

## Status Flags

| Flag | Active For | Logic | Meaning |
|------|-----------|-------|---------|
| Z (Zero) | All ops | NOR of all C[i] | Result equals zero |
| N (Sign) | All ops | C3 (MSB of result) | Result is negative (signed) |
| C (Carry) | Op 6, 7, 8 | Adder COUT or shift MSB | Carry-out or shifted-out bit |
| V (Overflow) | Op 6, 7 | (A3 ⊕ C3) ∧ (B′3 ⊕ C3) | Signed arithmetic overflow |

---

## Project Structure

```
ALU_Projesi/
│
├── TopLevel.asc / .asy          # Top-level ALU schematic
│
├── Transistor-level gates
│   ├── NOT1.asc / .asy
│   ├── NAND2.asc / .asy
│   └── NOR2.asc / .asy
│
├── Derived 2-input gates
│   ├── AND2.asc / .asy
│   ├── OR2.asc / .asy
│   ├── XOR2.asc / .asy
│   └── XNOR2.asc / .asy
│
├── 4-bit logic blocks
│   ├── NOT4.asc / .asy
│   ├── NAND4.asc / .asy
│   ├── NOR4.asc / .asy
│   ├── XOR4.asc / .asy
│   └── XNOR4.asc / .asy
│
├── Arithmetic blocks
│   ├── FullAdder.asc / .asy
│   ├── Adder4bit.asc / .asy
│   └── AddSub4bit.asc / .asy
│
├── Shift & Multiplexer
│   ├── Shift4bit.asc / .asy
│   ├── MUX2to1.asc / .asy
│   ├── MUX4to1.asc / .asy
│   ├── MUX8to1.asc / .asy
│   └── MUX8to1_4bit.asc / .asy
│
├── Sequential & Status
│   ├── DLatch.asc / .asy
│   ├── dFlipFlop.asc / .asy
│   ├── Reg4Bit.asc / .asy
│   └── StatusFlags.asc / .asy
│
└── Test benches
    ├── test_TopLevel.asc
    ├── test_Adder4bit.asc
    ├── test_AddSub4bit.asc
    ├── test_FullAdder.asc
    ├── test_MUX2to1.asc
    ├── test_MUX4to1.asc
    ├── test_Shift4bit.asc
    ├── test_AND.asc
    ├── test_OR.asc
    ├── test_NOT.asc
    ├── test_XOR.asc
    ├── test_XNOR.asc
    └── Draft7.asc
```

---

## Design Hierarchy

| Level | Block | Built From | Function |
|-------|-------|-----------|----------|
| Transistor | NOT1, NAND2, NOR2 | PMOS + NMOS | Universal CMOS gates |
| Gate | AND2, OR2, XOR2, XNOR2 | NAND/NOR + NOT | Derived 2-input gates |
| 4-bit Logic | NOT4, NAND4, NOR4, XOR4, XNOR4 | 4 × 2-input gate | Bitwise 4-bit operations |
| Arithmetic | FullAdder, Adder4bit, AddSub4bit | XOR + AND + OR | Add/subtract with carry chain |
| Shift | Shift4bit | Buffered wiring | Left shift by 1, MSB out |
| Multiplexer | MUX2to1 → MUX8to1_4bit | Hierarchical AND/OR/NOT | Selects one of 8 op results |
| Status | StatusFlags | Logic gates + MUX | Z, N, C, V flags |
| Sequential | DLatch, dFlipFlop, Reg4Bit | NAND-based master-slave | 4-bit clocked storage |
| Top | TopLevel | All blocks | Complete 4-bit ALU |

---

## How to Run

1. Install **LTspice XVII** or newer (free from [Analog Devices](https://www.analog.com/en/design-center/design-tools-and-calculators/ltspice-simulator.html))
2. Clone this repository
3. Open `TopLevel.asc` in LTspice to view the full ALU
4. Open any `test_*.asc` file and press **Run** to simulate individual blocks
5. Use the waveform viewer to inspect outputs and status flags

---

## Report

The full midterm project report is available in [`ALU_Midterm_Report.docx`](./ALU_Midterm_Report.docx).

---

## License

This project was submitted as academic coursework. Feel free to use it as a reference for educational purposes.
