## **Check Physical Volumes (PVs)**

**Command**: `sudo pvs`
- Displays a concise overview of physical volumes, including their size, volume group, and free space.
- Example output:
  ```
  PV             VG   Fmt  Attr PSize  PFree
  /dev/nvme0n1p1 bfvg lvm2 a--   1.86t      0
  /dev/nvme1n1   bfvg lvm2 a--  <3.64t <39.62g
  /dev/nvme2n1p2 bfvg lvm2 a--  <3.64t      0
  ```

**Command**: `sudo pvdisplay </dev/nvme1n1>`
- Provides detailed information about each physical volume, such as UUID, size, and allocation status.
- Example output:
  ```
  --- Physical volume ---
  PV Name               /dev/nvme1n1
  VG Name               bfvg
  PV Size               <3.64 TiB / not usable <3.84 MiB
  Allocatable           yes
  PE Size               4.00 MiB
  Total PE              953861
  Free PE               10142
  Allocated PE          943719
  PV UUID               N00zye-M0Oh-q0OV-harc-eOQ5-GXSX-XBybVa
  ```

## **Check Volume Groups (VGs)**

**Command**: `sudo vgs`
- Shows a summary of volume groups, including the number of physical and logical volumes, size, and free space.
- Example output:
  ```
  VG        #PV #LV #SN Attr   VSize   VFree
  bfvg        3   1   0 wz--n- 100.00g 20.00g
  ```

**Command**: `sudo vgdisplay <bfvg>`
- Displays detailed information about volume groups, including metadata, extent size, and allocation policy.
- Example output:
  ```
  --- Volume group ---
  VG Name               bfvg
  System ID
  Format                lvm2
  Metadata Areas        3
  Metadata Sequence No  6
  VG Access             read/write
  VG Status             resizable
  MAX LV                0
  Cur LV                1
  Open LV               1
  Max PV                0
  Cur PV                3
  Act PV                3
  VG Size               <9.14 TiB
  PE Size               4.00 MiB
  Total PE              2395972
  Alloc PE / Size       2385830 / 9.10 TiB
  Free  PE / Size       10142 / <39.62 GiB
  VG UUID               pjwWD3-JTnU-UHQF-sitl-wMJZ-ILVz-jiXT8S
  ```

## **Check Logical Volumes (LVs)**

**Command**: `sudo lvs`
- Lists logical volumes with details like size, volume group, and attributes.
- Example output:
  ```
  LV   VG   Attr       LSize Pool Origin Data%  Meta%  Move Log Cpy%Sync Convert
  bflv bfvg -wi-ao---- 9.10t
  ```

**Command**: `sudo lvdisplay`
- Provides detailed information about logical volumes, including their path, size, and status.
- Example output:
  ```
  --- Logical volume ---
  LV Path                /dev/bfvg/bflv
  LV Name                bflv
  VG Name                bfvg
  LV UUID                DZWbta-GCdW-oFjR-C1OV-rOXS-uRKd-2fuQlj
  LV Write Access        read/write
  LV Creation host, time SC2-Builder-03, 2025-06-30 09:28:47 +0800
  LV Status              available
  # open                 1
  LV Size                9.10 TiB
  Current LE             2385830
  Segments               3
  Allocation             inherit
  Read ahead sectors     auto
  - currently set to     256
  Block device           253:0
  ```

## **Check LVM Configuration File**

**Command**: `cat /etc/lvm/lvm.conf`
- View the configuration file to check settings like metadata backups, logging, or device filters.
- Example: `grep -v "^#" /etc/lvm/lvm.conf | grep -v "^$" | less`
  - This filters out comments and empty lines for clarity.

**Command**: `lvmconfig`
- Displays the current LVM configuration, including default and overridden settings.
- Example: `sudo lvmconfig --type full`

## **Check LVM Version**

**Command**: `lvm version`
- Displays the LVM2 version and related library versions.
- Example output:
  ```
  LVM version:     2.03.11(2) (2021-01-08)
  Library version: 1.02.177 (2021-01-08)
  Driver version:  4.45.0
  ```

## **Check LVM Status**

**Command**: `sudo lvmdiskscan`
- Scans for devices that can be used as physical volumes.

**Command**: `lvs -o+seg_monitor`
- If using LVM with thin provisioning or snapshots, this shows monitoring status for logical volumes.

## **Check Mounted Logical Volumes**

**Command**: `df -h`
- Lists mounted filesystems, including those on logical volumes.
- Example output:
  ```
  Filesystem                 Size  Used Avail Use% Mounted on
  /dev/mapper/bfvg-bflv      9.0T  559G  8.1T   7% /
  ```

**Command**: `lsblk`
- Displays a tree view of block devices, including LVM logical volumes and their mount points.
- Example output:
  ```
  NAME                  MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
  nvme0n1       259:0    0   1.9T  0 disk
  └─nvme0n1p1   259:1    0   1.9T  0 part
    └─bfvg-bflv 253:0    0   9.1T  0 lvm  /
  nvme1n1       259:2    0   3.7T  0 disk
  └─bfvg-bflv   253:0    0   9.1T  0 lvm  /
  nvme2n1       259:3    0   3.7T  0 disk
  ├─nvme2n1p1   259:4    0   512M  0 part /boot/efi
  └─nvme2n1p2   259:5    0   3.7T  0 part
    └─bfvg-bflv 253:0    0   9.1T  0 lvm  /
  ```

## **Check LVM Metadata**

**Command**: `pvck /dev/sda1`
- Checks the metadata on a physical volume for consistency.

**Command**: `vgck my_vg`
- Checks the consistency of a volume group’s metadata.
