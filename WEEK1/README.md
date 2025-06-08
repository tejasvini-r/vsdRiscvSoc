
  
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
```

