
  
# 🚀 VSD SoC Labs — Week 1: RISC-V Toolchain & Debugging

🌟 *Goal:* Set up a RISC-V environment, compile and analyze C programs, explore the ABI, debug with GDB, boot bare-metal ELF on QEMU, use inline assembly, perform memory-mapped I/O, implement a custom linker script, understand start-up code, handle interrupts, and compare RV32IMAC vs RV32IMC.

## Task 1: Install & Sanity-Check the Toolchain

This section provides step-by-step instructions to unpack the `riscv-toolchain-rv32imac-x86_64-ubuntu.tar.gz` file, add the toolchain to your PATH, and verify that the `gcc`, `objdump`, and `gdb` binaries are functioning correctly on an Ubuntu system.

### Step 1: Unpack the Toolchain
The downloaded file is a compressed tarball. To extract it, follow these steps:

- Open a terminal.
- Navigate to the directory containing the tarball (e.g., `~/Downloads`):
  ```bash
  cd ~/Downloads
  ```
- Extract the tarball to your home directory (or another preferred location, e.g., `~/riscv`):
  ```bash
  tar -xzf riscv-toolchain-rv32imac-x86_64-ubuntu.tar.gz -C ~/
  ```
  This will create a directory (e.g., `~/riscv` or a similar name based on the tarball's contents) containing the toolchain.

### Step 2: Add the Toolchain to PATH
To make the toolchain binaries accessible from any terminal session, add them to your PATH environment variable:

- Open your `~/.bashrc` file in a text editor (e.g., `nano` or `vim`):
  ```bash
  nano ~/.bashrc
  ```
- Add the following line at the end of the file, adjusting the path if the toolchain directory differs (e.g., replace `~/riscv/bin` with the actual path to the `bin` directory):
  ```bash
  export PATH=$HOME/riscv/bin:$PATH
  ```
- Save and close the file (in `nano`, press `Ctrl+O`, `Enter`, then `Ctrl+X`).
- Apply the changes to the current terminal session:
  ```bash
  source ~/.bashrc
  ```

### Step 3: Verify the Toolchain Binaries
To confirm that the `gcc`, `objdump`, and `gdb` binaries are working, run the following commands:

- Check the RISC-V GCC version:
  ```bash
  riscv32-unknown-elf-gcc --version
  ```
  Expected output: Version information, e.g., `riscv32-unknown-elf-gcc (GCC) 10.2.0` (version may vary).

- Check the RISC-V objdump version:
  ```bash
  riscv32-unknown-elf-objdump --version
  ```
  Expected output: Version information, e.g., `GNU objdump (GNU Binutils) 2.35` (version may vary).

- Check the RISC-V GDB version:
  ```bash
  riscv32-unknown-elf-gdb --version
  ```
  Expected output: Version information, e.g., `GNU gdb (GDB) 9.2` (version may vary).

If all commands return version information without errors, the toolchain is correctly installed and accessible.

### Troubleshooting
- **Command not found**: Ensure the `bin` directory (e.g., `~/riscv/bin`) exists and contains the binaries. Verify the PATH setting with:
  ```bash
  echo $PATH
  ```
- **Permission issues**: Ensure the binaries have executable permissions:
  ```bash
  chmod +x ~/riscv/bin/*
  ```
- **Wrong architecture**: Confirm the toolchain matches your system (x86_64 Ubuntu in this case).

### Notes
- Replace `~/riscv/bin` with the actual path if the tarball extracts to a different directory.
- To make the PATH change temporary (for the current session only), run:
  ```bash
  export PATH=$HOME/riscv/bin:$PATH
  ```
  without editing `~/.bashrc`.
- For a system-wide installation, consider extracting to `/opt/riscv` and updating `/etc/environment` (requires sudo).

This setup ensures your RISC-V toolchain is ready for compiling, debugging, and analyzing RISC-V programs.


![Image](https://github.com/user-attachments/assets/a7fd21f7-c766-4ee1-9252-49d7daab410a)

![Image](https://github.com/user-attachments/assets/0cddb854-26c0-4563-838f-ec2a29600be5)

![Image](https://github.com/user-attachments/assets/7883ca29-d17d-4fac-bc18-4fee3aca2c4c)



# **Task 2: Compile "Hello, RISC-V"**

This section provides instructions to write a minimal "Hello, RISC-V" C program, cross-compile it for RV32IMC using the RISC-V toolchain, and verify the output ELF file.

---

## **Step 1: Write the Minimal C Program**

**Create a file named `hello.c` with the following one-line program:**

Open a text editor (e.g., nano or vim) and create `hello.c`:

```
nano hello.c
```

**Add the following code:**

```
#include <stdio.h>
int main() { printf("Hello, RISC-V\n"); return 0; }
```

**Save and close the file** (in nano: press `Ctrl+O`, Enter, then `Ctrl+X`).

---

## **Step 2: Cross-Compile for RV32IMC**

**Compile the program using the RISC-V GCC toolchain with specific flags to target RV32IMC:**

```
riscv32-unknown-elf-gcc -march=rv32imc -mabi=ilp32 -o hello.elf hello.c
```

### **Explanation of Flags**

* `-march=rv32imc`: Targets the RV32IMC architecture (32-bit RISC-V with Integer, Multiplication, and Compressed instructions).
* `-mabi=ilp32`: Uses the ILP32 ABI (32-bit integers, longs, and pointers).
* `-o hello.elf`: Specifies the output file name as `hello.elf`.
* `hello.c`: The input source file.

---

## **Step 3: Verify the ELF File**

**Confirm that the compiled `hello.elf` is a 32-bit RISC-V ELF file:**

```
file hello.elf
```

### **Expected Output**

```
hello.elf: ELF 32-bit LSB executable, RISC-V, version 1 (SYSV), statically linked, not stripped
```

---

## **Troubleshooting**

* **Compiler not found:**
  Ensure the RISC-V toolchain is in your `PATH`. You can check with:

  ```
  echo $PATH
  ```

* **Architecture mismatch:**
  The `-march=rv32imc` flag ensures targeting RV32IMC.
  If errors occur, confirm your toolchain supports RV32IMC (usually `rv32imac` includes `rv32imc`).

* **`file` command not found:**
  Install the `file` utility on Ubuntu:

  ```
  sudo apt update && sudo apt install file
  ```

---

## **Notes**

* The program is minimal and uses `stdio.h` for `printf`.
  For a **bare-metal setup** (no standard library), you’ll need a custom `printf` implementation — covered in later tasks.

* The resulting `hello.elf` is **statically linked by default**, suitable for:

  * Debugging
  * Running in **QEMU**
  * Loading onto RISC-V hardware boards
  * 
![Image](https://github.com/user-attachments/assets/155cacc2-1e28-4054-9be6-79542966174e)

![Image](https://github.com/user-attachments/assets/3f54212a-3c64-4335-a105-e518003229fc)


# **Task 3: From C to Assembly**

This task demonstrates how to generate the assembly file from a C program and briefly explains the function prologue and epilogue.

---

## **Step 1: Generate the Assembly File**

Use the `-S` flag with the compiler to generate an assembly `.s` file from `hello.c`:

```
riscv32-unknown-elf-gcc -S -O0 hello.c
```

* `-S`: Tells the compiler to generate assembly instead of object code.
* `-O0`: Disables optimizations to keep the output simple and readable.

This will generate a file named `hello.s` in the same directory.

---

## **Step 2: Understand Prologue and Epilogue**

In the `hello.s` file, locate the `main` function. You may find lines like:

```
  addi    sp,sp,-16
  sw      ra,12(sp)
  sw      s0,8(sp)
  addi    s0,sp,16
```

### **Explanation - Function Prologue**

* `addi sp, sp, -16`: Allocates 16 bytes on the stack for this function.
* `sw ra, 12(sp)`: Saves the return address (from `ra`) at offset 12 of the stack.
* `sw s0, 8(sp)`: Saves the frame pointer (old `s0`) at offset 8 of the stack.
* `addi s0, sp, 16`: Sets up the new frame pointer.

These steps are required for stack management and to preserve the calling function’s context.

### **Explanation - Function Epilogue**

At the end of the function, you may see:

```
  lw      ra,12(sp)
  lw      s0,8(sp)
  addi    sp,sp,16
  ret
```

* `lw ra, 12(sp)`: Restores the return address.
* `lw s0, 8(sp)`: Restores the previous frame pointer.
* `addi sp, sp, 16`: Deallocates the stack space.
* `ret`: Returns to the calling function.

This ensures the function exits cleanly and returns to the correct location.

![Image](https://github.com/user-attachments/assets/5b02f476-2da0-4d85-927d-f57cce7e2e8d)


# **Task 4: Hex Dump & Disassembly**

This task demonstrates how to generate a disassembly dump and a raw Intel HEX format file from a compiled RISC-V ELF file. It also explains the components of a disassembled instruction.

---

## **Step 1: Generate a Disassembly Dump**

Use `riscv32-unknown-elf-objdump` to disassemble your ELF file:

```
riscv32-unknown-elf-objdump -d hello.elf > hello.dump
```

* `-d`: Disassembles the contents of the executable sections.
* `> hello.dump`: Redirects the output to a file.

This will create a human-readable disassembly of `hello.elf`.

---

## **Step 2: Generate Intel HEX Format File**

Use `riscv32-unknown-elf-objcopy` to convert the ELF file into Intel HEX format:

```
riscv32-unknown-elf-objcopy -O ihex hello.elf hello.hex
```

* `-O ihex`: Sets the output format to Intel HEX.
* `hello.hex`: Output file name containing the hex dump.

---

## **Step 3: Understand the Disassembly Output**

Each line in the disassembly typically includes:

```
  address:   opcode    mnemonic   operands
```

Example:

```
  00000000 <main>:
     0: 1141        addi    sp,sp,-16
```

* **Address (e.g., `0`)**: Memory location of the instruction.
* **Opcode (e.g., `1141`)**: Hexadecimal encoding of the machine instruction.
* **Mnemonic (e.g., `addi`)**: Human-readable name of the instruction.
* **Operands (e.g., `sp,sp,-16`)**: Registers and immediate values used by the instruction.

This helps in understanding how the compiler translates C code into low-level instructions.

![Image](https://github.com/user-attachments/assets/ea2c4664-7469-41f5-9fc1-4cf9b6f250a9)

![Image](https://github.com/user-attachments/assets/4079ee21-5e42-4c6f-96cb-d3f776ff03f2)

![Image](https://github.com/user-attachments/assets/b713e322-e93d-4e5f-8b7a-9179362c44f4)


# **Task 5: ABI & Register Cheat-Sheet**

This task summarizes the 32 integer registers in the RV32I architecture, including their ABI names and roles in the calling convention.

---

## **Register Mapping Table**

| Register | ABI Name | Description                      |
| -------- | -------- | -------------------------------- |
| x0       | zero     | Constant zero                    |
| x1       | ra       | Return address                   |
| x2       | sp       | Stack pointer                    |
| x3       | gp       | Global pointer                   |
| x4       | tp       | Thread pointer                   |
| x5       | t0       | Temporary                        |
| x6       | t1       | Temporary                        |
| x7       | t2       | Temporary                        |
| x8       | s0/fp    | Saved register / frame pointer   |
| x9       | s1       | Saved register                   |
| x10      | a0       | Function argument / return value |
| x11      | a1       | Function argument / return value |
| x12      | a2       | Function argument                |
| x13      | a3       | Function argument                |
| x14      | a4       | Function argument                |
| x15      | a5       | Function argument                |
| x16      | a6       | Function argument                |
| x17      | a7       | Function argument                |
| x18      | s2       | Saved register                   |
| x19      | s3       | Saved register                   |
| x20      | s4       | Saved register                   |
| x21      | s5       | Saved register                   |
| x22      | s6       | Saved register                   |
| x23      | s7       | Saved register                   |
| x24      | s8       | Saved register                   |
| x25      | s9       | Saved register                   |
| x26      | s10      | Saved register                   |
| x27      | s11      | Saved register                   |
| x28      | t3       | Temporary                        |
| x29      | t4       | Temporary                        |
| x30      | t5       | Temporary                        |
| x31      | t6       | Temporary                        |

---

## **Calling Convention Summary**

* **a0–a7 (x10–x17)**: Used for passing arguments to functions and returning values.
* **s0–s11 (x8–x9, x18–x27)**: Callee-saved registers; preserved across function calls.
* **t0–t6 (x5–x7, x28–x31)**: Caller-saved registers; may be overwritten by function calls.

This cheat-sheet helps in understanding register usage during function calls and stack frame setup in RISC-V.

![Image](https://github.com/user-attachments/assets/98621c5a-9614-4b50-9f5d-774bc16f3c47)

![Image](https://github.com/user-attachments/assets/3c864520-162b-4b77-994a-c4586bfd8086)

# **Task 6: Stepping with GDB**

This task shows how to debug a RISC-V ELF file using `riscv32-unknown-elf-gdb`, set breakpoints, step through execution, and inspect registers.

---

## **Step 1: Launch GDB and Load the ELF**

Run the following command to start GDB with your ELF file:

```
riscv32-unknown-elf-gdb hello.elf
```

---

## **Step 2: Enter Simulation Mode**

At the GDB prompt, enter:

```
(gdb) target sim
```

This sets the target to the built-in GDB simulator for RISC-V.

---

## **Step 3: Set a Breakpoint at `main`**

```
(gdb) break main
```

This sets a breakpoint at the beginning of the `main` function.

---

## **Step 4: Run the Program**

```
(gdb) run
```

Execution will stop at the `main` function.

---

## **Step 5: Step Through Instructions**

Use `stepi` or `nexti` to step through instructions:

```
(gdb) stepi
```

---

## **Step 6: Inspect Registers**

To view the contents of registers like `a0`, run:

```
(gdb) info reg a0
```

You can inspect all registers with:

```
(gdb) info registers
```

---

## **Step 7: Disassemble Code**

To view disassembled instructions at the current location:

```
(gdb) disassemble
```

This helps you trace how instructions are executed and how data flows through registers.

![Image](https://github.com/user-attachments/assets/32f39d29-5634-4e16-af76-7b340e073e1c)

![Image](https://github.com/user-attachments/assets/b5d9863a-f7c4-4d3d-a0db-142aa4bd95c6)


# **Task 7: Running Under an Emulator**

This task demonstrates how to run a bare-metal RISC-V ELF file using software emulators such as Spike or QEMU, and how to observe UART output in the terminal.

---

## **Method 1: Using Spike (RISC-V ISA Simulator)**

Run your ELF using Spike:

```
spike --isa=rv32imc pk hello.elf
```

* `--isa=rv32imc`: Specifies the ISA variant (RV32IMC in this case).
* `pk`: Proxy kernel (must be available if used).
* `hello.elf`: Your compiled RISC-V executable.

> Note: Ensure `pk` (proxy kernel) is available in your environment for this method.

---

## **Method 2: Using QEMU (Quick Emulator)**

Alternatively, run the ELF using QEMU:

```
qemu-system-riscv32 -nographic -machine virt -kernel hello.elf
```

* `-nographic`: Disables graphical output and redirects UART to terminal.
* `-machine virt`: Uses a virtual board compatible with RISC-V.
* `-kernel hello.elf`: Loads the ELF file for execution.

---

## **UART Output**

If your program uses memory-mapped I/O to UART (e.g., writing to address `0x10000000`), you should see the output directly in the terminal.

> Ensure that your C or assembly code writes to the correct UART address for the target emulator’s memory map.

This method is useful for testing RISC-V programs in a simulated environment when physical hardware is not available.

![Image](https://github.com/user-attachments/assets/714b0b52-1dda-48c8-a805-aa3db1c56685)

![Image](https://github.com/user-attachments/assets/cdb084ee-0f9c-4b4f-948c-36e9e974bda4)

# **Task 8: Exploring GCC Optimisation**

This task explores how GCC optimization levels affect the generated assembly for a RISC-V program. It highlights differences between no optimization (`-O0`) and high optimization (`-O2`).

---

## **Step 1: Generate Assembly with -O0**

```
riscv32-unknown-elf-gcc -S -O0 hello.c -o hello_O0.s
```

* `-S`: Generates assembly code.
* `-O0`: No optimization (default for debugging).
* `hello.c`: Input source file.
* `-o hello_O0.s`: Output assembly file.

---

## **Step 2: Generate Assembly with -O2**

```
riscv32-unknown-elf-gcc -S -O2 hello.c -o hello_O2.s
```

* `-O2`: Enables common optimizations for speed without affecting debugging significantly.

---

## **Step 3: Compare and Analyze Differences**

Open both `.s` files side-by-side and look for changes in:

### **1. Dead-Code Elimination**

* `-O2` removes instructions for variables or code that do not affect the program result.

### **2. Register Allocation**

* Optimized code may use fewer memory accesses and more efficient register usage.

### **3. Function Inlining**

* Small functions may be inlined into the caller to avoid call overhead.

### **4. Reduced Prologue/Epilogue**

* `-O2` may reduce stack frame size and avoid saving/restoring unused registers.

---

## **Conclusion**

Compiler optimizations significantly affect performance and code size. Use `-O0` for debugging and higher levels like `-O2` or `-O3` for production or performance-critical code.
![Image](https://github.com/user-attachments/assets/642bd115-759d-48e5-ad02-a4c7527d4765)

![Image](https://github.com/user-attachments/assets/5fd4b56d-5cff-441e-8234-7cb5d9fbacb6)


# **Task 9: Inline Assembly Basics**

This task introduces inline assembly in C for RISC-V by accessing the cycle counter using the `csrr` instruction to read CSR 0xC00.

---

## **Step 1: Implement the Inline Assembly Function**

```c
static inline uint32_t rdcycle(void) {
    uint32_t c;
    asm volatile ("csrr %0, cycle" : "=r"(c));
    return c;
}
```

This function reads the current cycle count from the RISC-V `cycle` CSR (CSR address 0xC00) and returns it.

---

## **Step 2: Explanation of Key Elements**

### **1. `asm volatile`**

* `asm`: Keyword to insert assembly code.
* `volatile`: Tells the compiler not to optimize away this instruction. It ensures the assembly is always emitted.

### **2. `"csrr %0, cycle"`**

* `csrr`: Reads a CSR value into a register.
* `%0`: Placeholder for the first output operand.

### **3. `"=r"(c)`**

* `=r`: Output constraint. `=` means output, and `r` specifies any general-purpose register.
* `(c)`: The C variable `c` where the result is stored.

This construct binds the assembly output to a C variable, allowing integration of low-level hardware instructions within high-level C code.

---

## **Use Cases**

* Measuring CPU cycles.
* Profiling performance.
* Accessing hardware features not available in standard C libraries.

Inline assembly gives you fine-grained control over hardware-specific features while maintaining portability and structure in your C programs.

![Image](https://github.com/user-attachments/assets/6dff01cb-be40-48be-bed4-bb000fc4a83d)


# **Task 10: Memory-Mapped I/O Demo**

This task demonstrates how to perform memory-mapped I/O in a bare-metal RISC-V environment by toggling a GPIO register located at a fixed memory address.

---

## **Step 1: C Snippet for GPIO Toggle**

```c
#include <stdint.h>

void toggle_gpio() {
    volatile uint32_t *gpio = (uint32_t *)0x10012000;
    *gpio = 0x1; // Set GPIO high
}
```

This function writes a value to the memory address `0x10012000`, which represents a GPIO output register in a typical RISC-V memory map.

---

## **Step 2: Explanation of Key Elements**

### **1. `volatile` Keyword**

* Tells the compiler that the value pointed to by `gpio` can change at any time and should not be optimized away.
* Prevents instruction reordering and elimination during optimization.

### **2. Memory Alignment**

* The address `0x10012000` is 4-byte aligned, which is valid for `uint32_t` access.
* Misaligned memory access can lead to faults or undefined behavior.

---

## **Use Cases**

* Toggling LEDs or controlling other peripherals via GPIO.
* Interacting with memory-mapped hardware like timers, UART, or SPI.

Memory-mapped I/O is fundamental in embedded and bare-metal systems, allowing direct communication with hardware using C pointers.


![Image](https://github.com/user-attachments/assets/2246608c-be95-4fbc-9b32-4376bfc62925)


# **Task 11: Linker Script 101**

This task explains how to write a minimal linker script for RV32IMC, placing code (.text) at Flash base (0x00000000) and data (.data) at SRAM base (0x10000000).

---

## **Step 1: Minimal Linker Script**

Create a file named `link.ld`:

```ld
SECTIONS
{
    .text 0x00000000 :
    {
        *(.text*)
    }

    .data 0x10000000 :
    {
        *(.data*)
    }
}
```

This places:

* The `.text` (code) section at address `0x00000000` (commonly mapped to Flash ROM).
* The `.data` (initialized data) section at address `0x10000000` (mapped to SRAM).

---

## **Step 2: Compile and Link with Custom Linker Script**

```bash
riscv32-unknown-elf-gcc -march=rv32imc -mabi=ilp32 -c hello.c -o hello.o
riscv32-unknown-elf-ld -T link.ld hello.o -o hello.elf
```

* `-T link.ld`: Specifies the custom linker script.
* The result is an ELF file with code and data mapped to appropriate memory addresses.

---

## **Step 3: Verify Memory Layout**

Use `objdump` to check section addresses:

```bash
riscv32-unknown-elf-objdump -h hello.elf
```

---

## **Flash vs SRAM Addressing**

* **Flash (0x00000000)**: Non-volatile memory used to store program code.
* **SRAM (0x10000000)**: Volatile memory for data that changes during execution.

Linker scripts are essential in bare-metal development for mapping program sections to hardware-specific memory locations.


![Image](https://github.com/user-attachments/assets/0a445700-48ec-45e7-86cd-3ca1a3abff45)


# Task 12: crt0.S in Bare-Metal RISC-V Programs

This task explains how to write a basic `crt0.S` file and its role in a bare-metal RISC-V (e.g., RV32IMC) application.

---

## **Step 1: Purpose of `crt0.S`**

`crt0.S` is startup code responsible for initializing the system before the C `main()` function is called. It is the entry point after reset.

---

## **Step 2: Responsibilities**

1. **Initialize Stack Pointer**
   Set the `sp` register using a symbol defined in the linker script (e.g., `_stack_top`).

2. **Clear .bss Section**
   Zero out the memory area reserved for uninitialized global/static variables.

3. **Copy .data Section** *(if needed)*
   Copy initialized data from ROM to RAM (not always included in minimal startup).

4. **Call `main()`**
   Hand over control to the C `main()` function.

5. **Handle Return from `main()`**
   If `main()` returns, loop infinitely or enter low-power mode.

---

## **Step 3: Minimal Example**

```assembly
.section .init
.globl _start
_start:
    la sp, _stack_top      # Step 1: Set up stack pointer

    la a0, _bss_start      # Step 2: Clear .bss section
    la a1, _bss_end
1:
    beq a0, a1, 2f
    sw zero, 0(a0)
    addi a0, a0, 4
    j 1b
2:

    call main              # Step 4: Call main()

1:
    j 1b                   # Step 5: Loop if main returns
```

---

## **Step 4: Linker Script Coordination**

Ensure your linker script defines the symbols `_stack_top`, `_bss_start`, and `_bss_end`. These guide stack allocation and memory clearing.

---

## **Step 5: Where to Get `crt0.S`**

* **Newlib C Library**: Contains generic startup files.
* **Vendor SDKs**: Device-specific startup code from SiFive, Andes, etc.
* **Open-source examples**:

  * [riscv-pk](https://github.com/riscv/riscv-pk)
  * [emb-riscv-examples](https://github.com/riscv-software-src/emb-riscv-examples)

---

## **Notes**

* Always match `crt0.S` with the memory layout from your linker script.
* Avoid hardcoding addresses unless absolutely necessary.

---

**Tags**: RISC-V, crt0, startup code, bare-metal, embedded systems

![Image](https://github.com/user-attachments/assets/95e3ca39-f7f4-4c98-b238-9f833533834a)

![Image](https://github.com/user-attachments/assets/ae4f04bd-34a2-4667-9b97-6e71329a21cf)

# Task 13: Interrupt Primer – Machine Timer Interrupt (MTIP)

This task demonstrates how to enable and handle the RISC-V machine-timer interrupt (MTIP) using C and assembly. Example output and disassembly logs are also included.

---

## **Step 1: Overview of MTIP**

The machine timer interrupt is triggered when the `mtime` register reaches the value in `mtimecmp`. It is typically used for periodic interrupts like system ticks.

---

## **Step 2: Enabling MTIP**

To enable the MTIP:

1. **Write to `mtimecmp`** – Set the next interrupt time.
2. **Enable MTIE bit in `mie`** – Allow machine timer interrupts.
3. **Enable MIE bit in `mstatus`** – Enable global interrupt handling.

```c
#define MTIME       (*(volatile uint64_t *)(0x0200BFF8))
#define MTIMECMP    (*(volatile uint64_t *)(0x02004000))

void enable_timer_interrupt() {
    MTIMECMP = MTIME + 10000; // Set next interrupt

    asm volatile("csrs mie, %0" :: "r"(0x80));    // Enable MTIE (bit 7)
    asm volatile("csrs mstatus, %0" :: "r"(0x8));  // Enable MIE (bit 3)
}
```

---

## **Step 3: Writing the Interrupt Handler**

There are two ways to write an ISR:

### **Option 1: Using `__attribute__((interrupt))`**

```c
void __attribute__((interrupt)) handle_mtimer(void) {
    MTIMECMP = MTIME + 10000; // Schedule next timer interrupt
    // Your periodic code here
}
```

### **Option 2: Naked Handler Pattern**

```c
void __attribute__((naked)) handle_mtimer(void) {
    asm volatile(
        "addi sp, sp, -16\n"
        "sw ra, 12(sp)\n"
        "sw t0, 8(sp)\n"
        "sw t1, 4(sp)\n"
        "sw t2, 0(sp)\n"
    );

    MTIMECMP = MTIME + 10000;

    asm volatile(
        "lw ra, 12(sp)\n"
        "lw t0, 8(sp)\n"
        "lw t1, 4(sp)\n"
        "lw t2, 0(sp)\n"
        "addi sp, sp, 16\n"
        "mret\n"
    );
}
```

---

## **Step 4: Set the Trap Vector**

Set the address of the interrupt handler in the `mtvec` register:

```c
void trap_init() {
    asm volatile("csrw mtvec, %0" :: "r"(handle_mtimer));
}
```

---

## **Step 5: Source Files for Task 13**

### `task13_timer_interrupt.c`

```c
extern void enable_timer_interrupt();
extern void trap_init();
extern volatile int interrupt_count;

int main() {
    interrupt_count = 0;
    trap_init();
    enable_timer_interrupt();

    while (1) {
        // Wait for interrupt to increment the counter
    }
}
```

### `interrupt_start.S`

```assembly
.section .text
.global _start

_start:
    lui sp, 0x10010        # Setup stack pointer
    mv sp, sp
    auipc t0, 0
    addi t0, t0, trap_handler - .
    csrw mtvec, t0         # Set mtvec to trap_handler
    jal main
    j .
```

### `interrupt.ld`

```ld
SECTIONS
{
    . = 0x00000000;
    .text : { *(.text*) }

    . = 0x10000000;
    .data : { *(.data*) }
    .bss  : { *(.bss*) }
}
```

### `build_timer_interrupt.sh`

```bash
#!/bin/bash

echo "=== Task 13: Timer Interrupt Implementation ==="
echo "1. Compiling startup and C files..."
riscv32-unknown-elf-gcc -c interrupt_start.S -o start.o
riscv32-unknown-elf-gcc -c task13_timer_interrupt.c -o main.o

echo "2. Linking with custom linker script..."
riscv32-unknown-elf-ld -T interrupt.ld start.o main.o -o task13_timer_interrupt.elf
echo "✓ Compilation successful!"

echo ""
echo "3. Verifying timer interrupt program:"
file task13_timer_interrupt.elf

echo ""
echo "4. Checking interrupt-related symbols:"
riscv32-unknown-elf-nm task13_timer_interrupt.elf | grep interrupt

echo ""
echo "5. CSR operations in disassembly:"
riscv32-unknown-elf-objdump -d task13_timer_interrupt.elf | grep -A20 '<_start>'
```

---

## **Step 6: Sample Output Verification**

```
=== Task 13: Timer Interrupt Implementation ===
1. Compiling startup and C files...
2. Linking with custom linker script...
✓ Compilation successful!

3. Verifying timer interrupt program:
task13_timer_interrupt.elf: ELF 32-bit LSB executable, UCB RISC-V, RVC, soft-float ABI, version 1 (SYSV), statically linked, not stripped

4. Checking interrupt-related symbols:
00000156 T enable_timer_interrupt
10000000 B interrupt_count
0000003a T timer_interrupt_handler
00000018 T trap_handler

5. CSR operations in disassembly:
00000000 <_start>:
    0:  lui sp,0x10010
    4:  mv sp,sp
    8:  auipc t0,0x0
    c:  addi t0,t0,16 # 18 <trap_handler>
   10:  csrw mtvec,t0
   14:  jal 208 <main>
   18:  j 16 <_start+0x16>

00000018 <trap_handler>:
   18:  addi sp,sp,-64
   1c:  sw ra,0(sp)
   20:  sw t0,4(sp)
   24:  sw t1,8(sp)
   28:  sw t2,12(sp)
   2c:  sw a0,16(sp)
...
```

---

## **Notes**

* Make sure the handler address in `mtvec` is aligned.
* Ensure global and local interrupt bits are correctly enabled.
* `mtime`/`mtimecmp` addresses depend on your SoC (common on CLINT).

---

**Tags**: RISC-V, interrupt, machine-timer, MTIP, embedded systems

![Image](https://github.com/user-attachments/assets/88d63c25-f309-4124-84b9-854815948841)

![Image](https://github.com/user-attachments/assets/6f173377-19f1-4bd9-9de5-92d861a7e7c5)

# Task 14: RV32IMAC vs RV32IMC – What’s the “A”?

This task explains the significance of the 'A' (Atomic) extension in the RV32IMAC instruction set and its use in embedded systems and OS development.

---

## **Step 1: RV32IMC vs RV32IMAC**

* **RV32IMC** includes the base instruction set (I), multiplication (M), and compressed (C) extensions.
* **RV32IMAC** includes all of the above **plus** the **Atomic (A)** extension.

---

## **Step 2: What is the 'A' Extension?**

The Atomic extension provides **atomic memory operations** (AMOs), which are essential for building synchronization primitives like locks, semaphores, and atomic counters.

### Key Instructions:

* `lr.w` – Load-Reserved word
* `sc.w` – Store-Conditional word
* `amoadd.w` – Atomic addition
* `amoxor.w`, `amoand.w`, `amoor.w` – Bitwise atomic operations
* `amoswap.w` – Atomic swap

These operations perform **read-modify-write** sequences safely in multi-core or interrupt-driven environments.

---

## **Step 3: Why Are Atomics Important?**

Without atomic instructions:

* Multi-core systems face race conditions when accessing shared data.
* Locks and other synchronization primitives become inefficient or error-prone.

With atomics:

* Implement **lock-free data structures**.
* Write **concurrent OS kernels**.
* Improve **performance and reliability** in embedded real-time systems.

---

## **Example: Spinlock Using AMO**

```c
volatile int lock = 0;

void acquire_lock() {
    while (__sync_lock_test_and_set(&lock, 1)) {
        // Busy-wait until lock is released
    }
}

void release_lock() {
    __sync_lock_release(&lock);
}
```

The compiler translates these intrinsics into `amoswap.w` or similar AMO instructions on RV32IMAC.

---

## **Toolchain and ISA Awareness**

* Ensure your compiler target (`-march=rv32imac`) includes the atomic extension.
* Linkers and libraries may depend on AMOs for thread-safe operations.

---

**Tags**: RISC-V, ISA extensions, RV32IMAC, atomic operations, embedded systems, concurrency

![Image](https://github.com/user-attachments/assets/ceb976f2-dd6f-47e1-9bfa-96486e6f4042)
