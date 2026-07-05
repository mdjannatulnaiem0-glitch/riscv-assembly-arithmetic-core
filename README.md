
# riscv-assembly-arithmetic-core
A lightweight RISC-V assembly core executing basic arithmetic operations (addition and subtraction). Designed to demonstrate low-level register manipulation, memory-mapped I/O concepts, and data hazard prevention in computer architecture.

# RISC-V Assembly Arithmetic Core

A high-performance, lightweight arithmetic unit implemented purely in **RISC-V Assembly Language (RV32I ISA)**. This project serves as a foundational microarchitecture blueprint, demonstrating how processors handle memory allocation, register tracking, and low-level arithmetic execution.

##  Key Architectural Features
- **Load-Store Architecture:** Implements standard RISC-V pseudo-instructions (`la`, `lw`, `sw`) to interact safely with the data segment.
- **Register Optimization:** Utilizes temporary (`t`) and saved (`s`) registers efficiently without causing structural or data hazards.
- **Modularity:** Structured with clear boundaries between the `.data` (memory initialization) and `.text` (instruction execution) segments.

##  The Assembly Code (`main.s`)

```assembly
.data
    # Allocating 32-bit words in memory for source operands and results
    num1:    .word 15       # Operand A
    num2:    .word 5        # Operand B
    res_add: .word 0        # Memory slot for Addition Result
    res_sub: .word 0        # Memory slot for Subtraction Result

.text
.globl main

main:
    # -----------------------------------------------------------------
    # 1. DATA LOADING STAGE
    # -----------------------------------------------------------------
    la t0, num1            # Load address of num1 into temporary register t0
    lw s1, 0(t0)           # Load value of num1 (15) into saved register s1
    
    la t1, num2            # Load address of num2 into temporary register t1
    lw s2, 0(t1)           # Load value of num2 (5) into saved register s2

    # -----------------------------------------------------------------
    # 2. ARITHMETIC EXECUTION STAGE
    # -----------------------------------------------------------------
    # Execution of Addition: R[s3] <- R[s1] + R[s2]
    add s3, s1, s2         # s3 = 15 + 5 = 20
    la t2, res_add         # Load target address for addition result
    sw s3, 0(t2)           # Store the sum back into memory (res_add)

    # Execution of Subtraction: R[s4] <- R[s1] - R[s2]
    sub s4, s1, s2         # s4 = 15 - 5 = 10
    la t3, res_sub         # Load target address for subtraction result
    sw s4, 0(t3)           # Store the difference back into memory (res_sub)

    # -----------------------------------------------------------------
    # 3. ENVIRONMENT EXIT STAGE
    # -----------------------------------------------------------------
    li a7, 10              # Load environment call code 10 (Exit program)
    ecall                  # System call to terminate execution cleanly
```

## 💻 How to Simulate and Verify
To verify the instruction pipeline and register state changes, you can use any standard RISC-V simulator.

1. Copy the code block from `main.s`.
2. Open an open-source visual simulator like **[Ripes](https://github.com)** or the web-based **[Venus RISC-V Simulator](https://github.io)**.
3. Paste the code into the Editor tab and click **Assemble**.
4. Step through the execution pipeline (`F5` or Step Button):
   - Monitor register `x19 (s3)` for the addition result: `0x00000014` (Hexadecimal for 20).
   - Monitor register `x20 (s4)` for the subtraction result: `0x0000000A` (Hexadecimal for 10).

##  Future Roadmap
- [ ] Implement Multi-bit Multiplication and Division logic blocks.
- [ ] Integrate interactive user input processing using `ecall` environment handlers.
- [ ] Implement conditional branch mechanisms for error handling (e.g., overflow detection).

## 📄 License
This project is open-source and available under the [MIT License](LICENSE).
