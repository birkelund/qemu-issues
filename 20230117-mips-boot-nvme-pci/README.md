# mips, nvme/pci boot regression (commit 145e2198d749)

Commit 145e2198d749 ("hw/mips/gt64xxx\_pci: Endian-swap using PCI\_HOST\_BRIDGE
MemoryRegionOps") broke my mips64 nvme boot test (little-endian host, mips64
and nvme boot device).

The pci device doesn't show up and the kernel panics.

    qemu-system-mips64 \
        -nodefaults -nographic -snapshot -no-reboot \
        -M "malta" -cpu "I6400" -m 512M \
        -nic user,model=pcnet \
        -drive file=images/rootfs.ext2,format=raw,if=none,id=d0 \
        -device nvme,serial=default,drive=d0 \
        -kernel images/vmlinux \
        -append "root=/dev/nvme0n1 console=ttyS0,115200" \
        -serial stdio
