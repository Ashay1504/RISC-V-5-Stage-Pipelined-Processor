# 5-Stage Pipelined RISC-V Processor

**SystemVerilog | RISC-V RV32I | RTL Design | Computer Architecture**

## Project Overview

This project implements pipeline hazard-control mechanisms for a 32-bit, five-stage, in-order RISC-V processor using SystemVerilog.

Developed as part of **ECE 3058: Computer Architecture at the Georgia Institute of Technology**, the project extends a provided processor RTL framework with data forwarding, pipeline stalling, and flushing mechanisms to manage instruction dependencies and control hazards.

The focus is on maintaining correct instruction execution while reducing unnecessary pipeline stalls through hardware-level dependency detection and operand forwarding.

## Processor Architecture

The processor follows the standard five-stage pipeline:

| Stage | Name | Function |
|---|---|---|
| IF | Instruction Fetch | Fetches instructions using the program counter |
| ID | Instruction Decode | Decodes instructions and reads register operands |
| EX | Execute | Performs ALU operations and resolves jump targets |
| MEM | Memory Access | Handles data memory operations |
| WB | Write Back | Writes results to the register file |

### Supported Instructions

The processor implements a subset of the RISC-V RV32I instruction set:

| Instruction | Operation |
|---|---|
| ADD | Register-to-register addition |
| ADDI | Addition with immediate |
| SUB | Register-to-register subtraction |
| LW | Load word from memory |
| SLT | Set less than |
| JAL | Jump and link |

## Key Features

### 1. Data Hazard Detection and Pipeline Stalling

Implemented hazard detection logic to identify register dependencies between instructions at different pipeline stages.

- Detects load-use dependencies.
- Introduces pipeline stalls when dependent data is unavailable.
- Handles register-file writeback/read conflicts.
- Coordinates pipeline control to prevent instructions from consuming stale register values.

**Relevant RTL:** `Stall_Control.sv`

### 2. Data Forwarding

Developed forwarding control logic to reduce unnecessary stalls when instruction results are available in later pipeline stages.

- Detects dependencies between source and destination registers.
- Selects forwarded values from EX/MEM and MEM/WB pipeline paths.
- Supports dependent arithmetic instruction execution through operand bypassing.

**Relevant RTL:** `FWD_Control.sv`

### 3. Control Hazard Handling

Implemented pipeline flushing logic for jump instructions.

- Supports JAL instructions resolved in the execute stage.
- Controls invalidation of instructions fetched along an incorrect execution path.
- Integrates flush control with the processor pipeline.

**Relevant RTL:** `Flush_Control.sv`

### 4. Processor Integration

Integrated hazard-control modules within the five-stage processor framework.

The implementation includes modules for instruction fetching, decoding, ALU execution, memory access, register-file operations, and writeback.

**Relevant RTL:** `Core.sv`

## Project Structure

```text
RISC-V-5-Stage-Pipelined-Processor/
├── include/
│   └── core_pkg.sv
├── ALU.sv
├── Core.sv
├── DFlipFlop.sv
├── DRAM.sv
├── EX_Stage.sv
├── FWD_Control.sv
├── Fetch.sv
├── Flush_Control.sv
├── ID_Stage.sv
├── IF_Stage.sv
├── Instr_Mem.sv
├── Mem_Stage.sv
├── Register_File.sv
├── Stall_Control.sv
├── WB_Stage.sv
└── README.md
```

## Verification and Testing

The processor was evaluated as part of the ECE 3058 pipelined processor laboratory.

The assignment's verification methodology checks processor functionality by observing register-file and memory state across execution cycles.

Important testing scenarios include:

- Load-use register dependencies
- Arithmetic data dependencies
- EX/MEM and MEM/WB forwarding
- Register-file writeback hazards
- Jump instruction flushing
- Instruction sequences containing multiple dependencies

**Course assessment:** 85/100 on the recorded Gradescope submission.

This score represents the course assessment rather than a claim of complete functional verification.

## Tools and Technologies

- **Hardware Description Language:** SystemVerilog
- **Architecture:** RISC-V RV32I subset
- **Design Concepts:** RTL design, pipelining, hazard detection, operand forwarding, pipeline stalls, and flushing
- **Verification Concepts:** Cycle-accurate simulation, register-file state checking, and instruction dependency testing

## Future Improvements

Potential extensions include:

- Developing additional self-checking SystemVerilog testbenches
- Adding assertions to verify forwarding and pipeline stall behavior
- Increasing coverage of instruction dependency scenarios
- Expanding instruction-set support
- Measuring pipeline CPI and stall-cycle overhead
- Investigating remaining corner cases in forwarding and hazard control

## Academic Acknowledgment

Developed for ECE 3058: Computer Architecture at the Georgia Institute of Technology.

The original processor framework and assignment specifications were provided as part of the course. The student's work focused on implementing and integrating the pipeline hazard-control mechanisms.

Any public distribution of course-provided RTL or assignment solutions is subject to the applicable course policies.
