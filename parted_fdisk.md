## 🧰 `parted` – Modern, scriptable partitioning tool

### 🔹 Start parted on a device
```bash
sudo parted /dev/sdX
```

### 🔹 View current partitions
```bash
(parted) print
```

### 🔹 Create a new partition table
```bash
(parted) mklabel gpt      # or msdos
```

### 🔹 Create a new partition
```bash
(parted) mkpart primary ext4 1MiB 1000MiB
```

### 🔹 Set partition type (for GPT)
```bash
(parted) set 1 boot on
```

### 🔹 Delete a partition
```bash
(parted) rm 1
```

### 🔹 Quit
```bash
(parted) quit
```

---

## 🧰 `fdisk` – Classic tool for MBR (msdos) partition tables

### 🔹 Start fdisk on a device
```bash
sudo fdisk /dev/sdX
```

### 🔹 View current partitions
```bash
Command (m for help): p
```

### 🔹 Create a new partition
```bash
Command (m for help): n
```

### 🔹 Delete a partition
```bash
Command (m for help): d
```

### 🔹 Change partition type
```bash
Command (m for help): t
```

### 🔹 Write changes to disk
```bash
Command (m for help): w
```

### 🔹 Quit without saving
```bash
Command (m for help): q
```


Would you like a cheat sheet image or script to automate some of these tasks?
