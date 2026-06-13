# 4-bit Custom ALU — LTspice VLSI Design Project

> **Course:** VLSI Design
> **Institution:** İstanbul Üniversitesi-Cerrahpaşa — Computer Engineering
> **Design Type:** Transistor-Level Digital Circuit Design
> **Tool:** LTspice

---

## Authors

* İsmail Çolak
* Yusuf Eren Aykurt

---

## Project Overview

This project presents a **4-bit custom Arithmetic Logic Unit (ALU)** designed entirely at the **transistor level** using **LTspice**.

Unlike standard digital simulations, this design does **not** use SPICE digital primitives or ready-made logic blocks. Every component — including logic gates, multiplexers, arithmetic units, registers, and status flag logic — is built using **PMOS and NMOS transistors**.

The ALU supports **8 different operations**, selected by a 3-bit opcode input `(S2, S1, S0)`. It includes:

* Two 4-bit input registers: **A** and **B**
* One 4-bit output register: **C**
* A 3-bit operation selector
* Four status flags: **Z, N, C, V**
* Fully transistor-level gate implementation
* Hierarchical LTspice schematics and symbols

---

## Main Features

* 4-bit custom ALU design
* Fully transistor-level CMOS implementation
* No built-in digital primitives
* 8 selectable operations
* 4-bit input and output registers
* Addition and subtraction support
* Left shift operation
* Zero, Sign, Carry, and Overflow flags
* Modular and hierarchical circuit structure
* Individual test benches for major blocks

---

## Supported Operations

| Op # | S2 | S1 | S0 | Operation    | Description                              |
| ---- | -- | -- | -- | ------------ | ---------------------------------------- |
| 1    | 0  | 0  | 0  | `NOT A`      | Bitwise complement of A                  |
| 2    | 0  | 0  | 1  | `NAND(A, B)` | Bitwise NAND operation                   |
| 3    | 0  | 1  | 0  | `NOR(A, B)`  | Bitwise NOR operation                    |
| 4    | 0  | 1  | 1  | `XOR(A, B)`  | Bitwise XOR operation                    |
| 5    | 1  | 0  | 0  | `XNOR(A, B)` | Bitwise XNOR operation                   |
| 6    | 1  | 0  | 1  | `A + B`      | 4-bit arithmetic addition                |
| 7    | 1  | 1  | 0  | `A - B`      | 4-bit subtraction using two's complement |
| 8    | 1  | 1  | 1  | `A << 1`     | Left shift by 1 bit, LSB filled with 0   |

---

## Status Flags

The ALU generates four status flags based on the selected operation and output result.

| Flag | Name          | Active For                   | Logic                              | Description                            |
| ---- | ------------- | ---------------------------- | ---------------------------------- | -------------------------------------- |
| `Z`  | Zero Flag     | All operations               | `NOR(C3, C2, C1, C0)`              | Set when the result is zero            |
| `N`  | Sign Flag     | All operations               | `C3`                               | Indicates the MSB of the result        |
| `C`  | Carry Flag    | Addition, subtraction, shift | Adder carry-out or shifted-out MSB | Indicates carry-out or shifted-out bit |
| `V`  | Overflow Flag | Addition, subtraction        | `(A3 ⊕ C3) ∧ (B′3 ⊕ C3)`           | Indicates signed arithmetic overflow   |

---

## Design Architecture

The project is designed using a bottom-up hierarchical approach. Basic transistor-level gates are first implemented, then used to build larger logic, arithmetic, sequential, and selection blocks.

| Level            | Block                                           | Built From                  | Purpose                      |
| ---------------- | ----------------------------------------------- | --------------------------- | ---------------------------- |
| Transistor Level | `NOT1`, `NAND2`, `NOR2`                         | PMOS and NMOS transistors   | Basic CMOS logic gates       |
| Gate Level       | `AND2`, `OR2`, `XOR2`, `XNOR2`                  | Basic gates                 | Derived 2-input logic gates  |
| 4-bit Logic      | `NOT4`, `NAND4`, `NOR4`, `XOR4`, `XNOR4`        | 4 parallel gate blocks      | Bitwise ALU operations       |
| Arithmetic       | `FullAdder`, `Adder4bit`, `AddSub4bit`          | XOR, AND, OR gates          | Addition and subtraction     |
| Shift Unit       | `Shift4bit`                                     | Buffered wiring             | Left shift operation         |
| Multiplexer      | `MUX2to1`, `MUX4to1`, `MUX8to1`, `MUX8to1_4bit` | Hierarchical MUX structure  | Selects ALU operation output |
| Sequential Logic | `DLatch`, `dFlipFlop`, `Reg4Bit`                | NAND-based storage elements | Input and output registers   |
| Status Logic     | `StatusFlags`                                   | Logic gates and MUX blocks  | Generates ALU flags          |
| Top Level        | `TopLevel`                                      | All major modules           | Complete 4-bit ALU system    |

---

## Project Structure

```text
ALU_Projesi/
│
├── TopLevel.asc / .asy
│   └── Complete top-level ALU schematic and symbol
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
├── Shift and multiplexer blocks
│   ├── Shift4bit.asc / .asy
│   ├── MUX2to1.asc / .asy
│   ├── MUX4to1.asc / .asy
│   ├── MUX8to1.asc / .asy
│   └── MUX8to1_4bit.asc / .asy
│
├── Sequential and status blocks
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

## How the ALU Works

The ALU takes two 4-bit inputs, `A[3:0]` and `B[3:0]`, and produces a 4-bit output `C[3:0]`.

1. Input values are stored in 4-bit registers.
2. Each operation block calculates its own result in parallel.
3. The 3-bit opcode `(S2, S1, S0)` selects one result through the 8-to-1 multiplexer.
4. The selected result is stored in the output register.
5. Status flag logic evaluates the result and generates `Z`, `N`, `C`, and `V`.

---

## Simulation Guide

### Requirements

* LTspice XVII or newer
* Windows operating system recommended
* Basic familiarity with LTspice schematic and waveform viewer

### Running the Project

1. Install **LTspice XVII** or a newer version.
2. Clone or download this repository.
3. Open `TopLevel.asc` in LTspice to view the complete ALU.
4. Open any `test_*.asc` file to simulate individual modules.
5. Press **Run** in LTspice.
6. Use the waveform viewer to inspect:

   * Input registers `A` and `B`
   * Output register `C`
   * Opcode selection lines `S2`, `S1`, `S0`
   * Status flags `Z`, `N`, `C`, and `V`

---

## Test Benches

The project includes several test benches for verifying each major module separately.

| Test Bench            | Purpose                              |
| --------------------- | ------------------------------------ |
| `test_TopLevel.asc`   | Simulates the complete ALU           |
| `test_Adder4bit.asc`  | Tests 4-bit addition                 |
| `test_AddSub4bit.asc` | Tests addition and subtraction       |
| `test_FullAdder.asc`  | Tests single-bit full adder behavior |
| `test_MUX2to1.asc`    | Tests 2-to-1 multiplexer             |
| `test_MUX4to1.asc`    | Tests 4-to-1 multiplexer             |
| `test_Shift4bit.asc`  | Tests left shift operation           |
| `test_AND.asc`        | Tests AND gate                       |
| `test_OR.asc`         | Tests OR gate                        |
| `test_NOT.asc`        | Tests inverter                       |
| `test_XOR.asc`        | Tests XOR gate                       |
| `test_XNOR.asc`       | Tests XNOR gate                      |

---

## Report

The full midterm project report is available here:

```text
ALU_Midterm_Report.docx
```

The report includes the design process, circuit explanations, module hierarchy, simulation results, and implementation details.

---

## Educational Purpose

This project was developed as part of the **VLSI Design** course at **İstanbul Üniversitesi-Cerrahpaşa, Computer Engineering Department**.

The main goal of the project is to understand how digital systems can be implemented from the transistor level upward, including:

* CMOS logic design
* Hierarchical circuit design
* Arithmetic circuit construction
* Register-based data storage
* ALU operation selection
* Status flag generation
* LTspice-based transistor-level simulation

---

## License

This project was submitted as academic coursework.

You may use this repository as a reference for educational and learning purposes. Please do not submit it directly as your own coursework.
