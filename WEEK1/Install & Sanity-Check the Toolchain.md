# **How to Unpack the RISC-V Toolchain, Add to PATH, and Verify**
1.Extract the tarball:

tar -xzf riscv-toolchain-rv32imac-x86_64-ubuntu.tar.gz

2.Move the extracted folder to a preferred location (optional):

sudo mv riscv-toolchain-rv32imac-x86_64-ubuntu /opt/riscv-toolchain

3.Add the toolchain binaries to your PATH environment variable:

export PATH=/opt/riscv-toolchain/bin:$PATH

4.To make the PATH change permanent, add the above line to your shell configuration file:

    For bash:

echo 'export PATH=/opt/riscv-toolchain/bin:$PATH' >> ~/.bashrc
source ~/.bashrc

For zsh:

    echo 'export PATH=/opt/riscv-toolchain/bin:$PATH' >> ~/.zshrc
    source ~/.zshrc

5.Confirm the toolchain binaries work by checking their versions:

riscv32-unknown-elf-gcc --version

riscv32-unknown-elf-objdump --version

riscv32-unknown-elf-gdb --version

![Screenshot from 2025-06-03 22-08-21](https://github.com/user-attachments/assets/62096787-0e93-43bb-8136-dbc915c985a8)
![Screenshot from 2025-06-03 22-09-04](https://github.com/user-attachments/assets/ec86e1ac-51a8-4e91-ae00-f725afcbe6a9)
![Screenshot from 2025-06-03 22-10-01](https://github.com/user-attachments/assets/fedb17ca-d06e-45e6-bea2-1d0dd5da3e24)

Add Week 1 task: Install & Sanity-Check the Toolchain

