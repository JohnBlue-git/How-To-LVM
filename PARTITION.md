## 🧭 What is a Partition in Linux?

A **partition** is a logical division of a physical storage device (like a hard drive or SSD). Each partition can hold a file system (e.g., ext4, xfs) and be mounted separately. Linux treats everything as a file, including partitions, which appear as `/dev/sda1`, `/dev/sdb2`, etc.

---

## 📦 Common Partition Types

### 🔹 Partition Table Types
- **MBR (msdos)**: Legacy format, supports up to 4 primary partitions.
- **GPT (GUID Partition Table)**: Modern format, supports unlimited partitions, required for disks >2TB.

### 🔹 Filesystem Types
- **ext4**: Most common Linux filesystem.
- **xfs**: High-performance, used in enterprise.
- **btrfs**: Advanced features like snapshots.
- **swap**: Used for virtual memory.
- **fat32 / ntfs**: For compatibility with Windows.

---

## 🛠 How to Set Partition Type

### ✅ Using `parted` (for GPT or MBR)

```bash
sudo parted /dev/sdX
(parted) mklabel gpt                # or msdos
(parted) mkpart primary ext4 1MiB 1000MiB
(parted) set 1 boot on              # set boot flag
(parted) quit
```

> Note: `parted` sets filesystem type during `mkpart`, but doesn't assign partition codes like `fdisk`.

---

### ✅ Using `fdisk` (for MBR)

```bash
sudo fdisk /dev/sdX
Command (m for help): n            # create new partition
Command (m for help): t            # change partition type
Partition number (1-4): 1
Hex code (type L to list codes): 82   # for swap, 83 for Linux
Command (m for help): w            # write changes
```

> Use `L` in `fdisk` to list all available partition type codes.

---

## 🧪 Example Partition Type Codes in `fdisk`

| Code | Type              |
|------|-------------------|
| 83   | Linux filesystem  |
| 82   | Linux swap        |
| 7    | HPFS/NTFS/exFAT   |
| b    | W95 FAT32         |
| ef   | EFI (GPT boot)    |
