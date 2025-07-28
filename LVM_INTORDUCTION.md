# LVM Introduction
Logical Volume Manager (LVM) is a powerful storage management system in Linux that provides flexible and dynamic control over disk space. LVM allows administrators to abstract physical storage into logical volumes, enabling easy resizing and management of filesystems without disrupting existing data. By organizing physical disks into pools and allocating storage in granular units, LVM simplifies disk space management, making it ideal for environments requiring scalable and adaptable storage solutions.
When disk space is insufficient (out of spaces), LVM’s Volume Groups (VGs) and Logical Volumes (LVs) allow seamless adjustments to storage capacity while preserving data integrity.

## Physical Volume (PV)  
Physical Volumes are the foundational block devices in LVM, such as entire disks or partitions, that serve as the raw storage for LVM operations.

## Volume Group (VG)  
Volume Groups aggregate multiple Physical Volumes into a single, unified storage pool. A VG is essentially a collection of PVs, providing a flexible container for allocating storage to Logical Volumes.

## Physical Extent (PE)  
Physical Extents are the smallest allocation units in LVM, with a size typically ranging from 4 KiB to 64 MiB (default is 4 MiB). The PE size is set during VG creation (e.g., `vgcreate -s 16M ...`) and applies uniformly across the VG.  
\
Historically, older LVM versions had stricter limits on the number of PEs a VG could contain, requiring larger PE sizes to fully utilize disk capacity. Modern LVM supports up to 2³² PEs (approximately 4 billion), allowing a VG to scale to massive sizes. For example, with a 4 MiB PE size, the theoretical maximum VG size is 4 MiB × 2³² ≈ 16 PiB (petabytes).  
\
The PE size does not limit the size of Logical Volumes (LVs), as LVs are built from multiple PEs. However, PE size impacts alignment and efficiency:  
- A small PE size (e.g., 4 KiB) increases overhead for large VGs.  
- A large PE size (e.g., 64 MiB) may waste space for small LVs.  
\
Choosing an appropriate PE size is a balance between flexibility and storage efficiency.

## Logical Volume (LV)  
Logical Volumes are created from the storage pool of a Volume Group. They act as virtual partitions that can be formatted and mounted as filesystems. LVs are typically named in the format `/dev/<vgname>/<lvname>`. By properly configuring VGs, LVs can be dynamically resized or reorganized to meet storage needs.

## Summary of LVM Structure  
The relationship between LVM components can be summarized as:  
**PE → PV → VG → LV**  
- **PE**: The smallest unit of storage allocation.  
- **PV**: Physical disks or partitions that provide raw storage.  
- **VG**: A pool of PVs that aggregates storage capacity.  
- **LV**: Logical partitions created from VGs, used as mountable filesystems.
