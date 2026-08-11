# 5G NR QC-LDPC Encoder

An FPGA-based implementation of a **Quasi-Cyclic Low-Density Parity-Check (QC-LDPC) encoder for 5G New Radio (5G NR)**.

The design implements **5G NR Base Graph 1 (BG1)** using Verilog RTL and targets the **AMD/Xilinx Kria KV260 FPGA platform**. Python scripts are used to preprocess the QC-LDPC base matrix and generate memory files required by the RTL implementation.

## Overview

Low-Density Parity-Check (LDPC) codes are used in the 5G NR physical layer for forward error correction of data channels.

5G NR uses structured **QC-LDPC codes**, where the parity-check matrix is constructed from smaller circulant permutation matrices. This structure makes the algorithm suitable for efficient hardware implementation using cyclic shifts and XOR operations.

This project implements the encoding operation in hardware and generates a valid LDPC codeword consisting of:

* Systematic information bits
* Generated parity bits

The resulting codeword satisfies the LDPC parity-check condition:

[
Hc^T = 0
]

where:

* (H) is the parity-check matrix
* (c) is the generated codeword

## Key Features

* 5G NR QC-LDPC encoding
* Base Graph 1 (BG1) support
* Configured for a lifting size greater than Z = 32
* Verilog RTL implementation
* Parallel cyclic-shift and XOR-based processing
* Hardware generation of parity blocks
* Python-based base-graph preprocessing
* Conversion of base-matrix data into FPGA-compatible `.mem` files
* Functional verification through RTL simulation
* FPGA implementation targeting Kria KV260
* Vivado-based synthesis and implementation flow

## Architecture

The QC-LDPC parity-check matrix is represented as a set of smaller circulant submatrices.

Instead of explicitly storing the complete expanded parity-check matrix, the encoder uses the shift values defined by the 5G NR base graph.

Each valid base-graph entry represents a cyclically shifted identity matrix.

The main encoding process consists of:

1. Receiving the systematic information blocks.
2. Reading the required cyclic-shift values from the BG1 configuration.
3. Performing cyclic shifts on the corresponding information blocks.
4. XORing the shifted blocks to calculate intermediate parity values.
5. Generating the required parity blocks.
6. Combining systematic and parity blocks to form the final LDPC codeword.

This approach avoids explicitly constructing the complete expanded parity-check matrix and is better suited for FPGA implementation.

## Project Flow

```text
5G NR BG1 Base Matrix
        |
        v
Python Preprocessing
        |
        v
Shift / Matrix .mem Files
        |
        v
Verilog QC-LDPC Encoder
        |
        v
RTL Simulation
        |
        v
Vivado Synthesis & Implementation
        |
        v
Kria KV260 FPGA
```

## Technologies Used

### Hardware Design

* Verilog HDL
* FPGA RTL Design
* Digital Logic Design

### FPGA Platform

* AMD/Xilinx Kria KV260

### Tools

* Xilinx Vivado
* Xilinx Vitis

### Software / Scripting

* Python
* C/C++

## Repository Structure

```text
5G-NR-QC-LDPC-Encoder/
│
├── rtl/
│   ├── ldpc_encoder.v
│   ├── parity_generation.v
│   ├── cyclic_shift.v
│   └── ...
│
├── tb/
│   ├── ldpc_encoder_tb.v
│   └── ...
│
├── python/
│   ├── base_graph_parser.py
│   ├── mem_generator.py
│   └── ...
│
├── mem/
│   ├── bg1.mem
│   └── ...
│
├── constraints/
│   └── ...
│
├── docs/
│   └── ...
│
└── README.md
```

The exact directory structure may vary depending on the current implementation.

## Encoding Principle

For a QC-LDPC matrix, each non-negative entry in the base graph corresponds to a cyclic permutation matrix.

For a lifting size (Z), a base-graph value (p) represents a cyclic shift by:

[
p \mod Z
]

The encoder therefore replaces large binary matrix multiplications with hardware-efficient:

* Cyclic shift operations
* XOR trees
* Structured parity-generation logic

This significantly reduces the complexity of implementing the expanded LDPC matrix directly.

## Verification

The RTL design is verified using simulation by applying information vectors to the encoder and checking the generated codeword.

A valid encoded codeword must satisfy:

[
Hc^T = 0
]

This provides a direct mathematical check that the generated parity bits are correct.

## FPGA Implementation

The RTL design is synthesized and implemented using Xilinx Vivado and targeted to the Kria KV260 platform.

The FPGA implementation demonstrates the mapping of the structured QC-LDPC encoding algorithm into parallel digital hardware.

## Future Improvements

Possible extensions include:

* Support for multiple lifting sizes
* Support for Base Graph 2
* Runtime-configurable lifting size
* AXI4-Stream interface
* Pipelined high-throughput architecture
* Resource and timing optimization
* Throughput and latency characterization
* Hardware/software control through the Zynq processing system

## Applications

* 5G NR physical-layer processing
* Forward Error Correction
* FPGA-based communication accelerators
* Digital communication hardware
* High-throughput channel coding

## Author

**Prajwal M**

Electronics and Communication Engineering
Digital VLSI | FPGA Design | RTL Design
