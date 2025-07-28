## How to set LVM (if the parition haven't set to LVM format)

Important Prerequisites
```bash
sudo apt install lvm2  # For Debian/Ubuntu
```

Below provide a practical sequence for setting up LVM to make /home extensible. The first and the most important things is to backup data. Becasue setting up LVM on nvme1n1 will require reformatting the partition (nvme1n1p1), which will erase all data on /home. Back up all files in /home to an external drive or another location before proceeding.
[How to backup and restore /home folder](./BACKUP_RESTORE.md)

Current Enviornment Recap
```bash
$ lsblk
nvme1n1     259:5    0   3.6T  0 disk
└─nvme1n1p1 259:6    0   3.6T  0 part /home
```

Unmount /home
```bash
sudo umount /home
```

Set Partition Type to 8e
```bash
$ fdisk /dev/nvme1n1
n  # Create new partition
t  # Change partition type
8e # Set to LVM format
w  # Write changes
```

Create Physical Volumes
```bash
$ sudo pvcreate /dev/nvme1n1p1
Physical volume "/dev/nvme1n1p1" successfully created.
```

Create Volume Groups
```bash
$ sudo vgcreate home_vg /dev/nvme1n1p1
Volume group "home_vg" successfully created
```

Create Logical Volumes
-L 3T: Specifies the size of the logical volume (3 terabytes). You can use less than the full 3.6TB to leave room for other logical volumes or snapshots in home_vg.
-n: Names the logical volume home_lv. Later, this will appear as /dev/mapper/home_vg-home_lv.
```bash
$ sudo lvcreate -L 3T -n home_lv home_vg
Logical volume "home_lv" created.
```

Format and Mount the Logical Volume
- mkfs.ext4: Formats the logical volume with the ext4 filesystem (or another filesystem like xfs if preferred).
- mount: Mounts the formatted logical volume at /home.
```bash
sudo mkfs.ext4 /dev/mapper/home_vg-home_lv
sudo mount /dev/mapper/home_vg-home_lv /home

# To make the mount persistent, update /etc/fstab
vim /etc/fstab
# Add a line like:
/dev/mapper/home_vg-home_lv  /home  ext4  defaults  0  2
# Save and verify:
sudo mount -a
```

## How to extend LV space (from existed disk or partition)

If using remaining space in home_vg (e.g., 0.6TB left)
- lvresize increases the logical volume size.
- resize2fs expands the ext4 filesystem to use the new space.
```bash
sudo lvresize -L +600G /dev/mapper/home_vg-home_lv
sudo resize2fs /dev/mapper/home_vg-home_lv
```

## How to extend LV space (from empty disk partition)

If adding a new disk (e.g., nvme2n1)
- vgextend adds the new disk to the volume group, increasing its capacity.
- Then, lvresize and resize2fs extend /home.
```bash
sudo pvcreate /dev/nvme2n1
sudo vgextend home_vg /dev/nvme2n1
sudo lvresize -L +3T /dev/mapper/home_vg-home_lv
sudo resize2fs /dev/mapper/home_vg-home_lv
```
