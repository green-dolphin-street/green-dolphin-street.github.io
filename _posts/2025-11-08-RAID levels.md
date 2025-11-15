---
title: "RAID Levels"
layout: single
date: 2025-11-08
categories:
  - IT Infrastructure Engineering
tags:
  - Storage
  - Availability
  - RAID
use_math: true
---

### RAID 0 (Striping):
* **How it works:** Data is broken into "chunks" (blocks) and written sequentially across all drives in the array (a process called "striping"). For example, Block 1 goes to Drive 1, Block 2 to Drive 2, Block 3 to Drive 1, etc. This allows the system to read and write using the combined speed of all drives at once.
* **Key Strength:** **Maximum performance** (both read and write) and 100% storage capacity.
* **Key Weakness:** **Zero fault tolerance.** If one drive fails, all data in the array is lost. The risk of failure increases with every drive added.
* **Parity:** None.
* **Redundancy:** None.

### RAID 1 (Mirroring):
* **How it works:** Creates an exact, real-time copy (a "mirror") of all data. Every single piece of data written to the array is simultaneously written to *all* drives in the set (minimum 2). The array appears as a single drive to the OS, but the drives are identical copies.
* **Key Strength:** **High fault tolerance.** The array can survive all but one drive failing. Good read performance.
* **Key Weakness:** **Poor storage efficiency.** You lose 50% of your total capacity (e.g., two 1TB drives = 1TB usable). Write speeds are slower.
* **Parity:** None. Redundancy is a direct copy.
* **Redundancy:** Excellent (1:1 copy).

### RAID 5 (Striping with Parity):
* **How it works:** Data is striped across all drives (like RAID 0), but for each stripe, a "parity block" is calculated (using XOR). This parity block is also written to one of the drives. The *location* of this parity block is rotated across all drives (e.g., on Drive 3 for the first stripe, Drive 2 for the second) to prevent a "parity bottleneck" on a single drive.
* **Key Strength:** **Good balance.** It offers fault tolerance while being very storage-efficient (you only lose the capacity of one drive). Read speeds are fast.
* **Key Weakness:** **Slow write performance** (due to the "read-modify-write" parity calculation). Long rebuild times if a drive fails, during which the array is vulnerable.
* **Parity:** Yes (Single XOR).
* **Parity & Restoration Example (4-bit):**
    * The logic is `A XOR B = Parity`. This is reversible: `A XOR Parity = B`.
    * **Writing Data (3 drives):**
        * Drive 1 (Data A): `1011`
        * Drive 2 (Data B): `0110`
        * Drive 3 (Parity): `1011 XOR 0110 = 1101`
    * **A Drive Fails (Drive 1):**
        * Drive 1: **FAIL**
        * Drive 2: `0110`
        * Drive 3: `1101`
    * **Restoration:** The controller calculates `Data B XOR Parity`:
        * `0110 XOR 1101 = 1011`
        * The missing `Data A` (`1011`) is perfectly rebuilt. This array can survive *one* drive failure.

### RAID 6 (Striping with Dual Parity):
* **How it works:** This is an enhanced version of RAID 5. It stripes data across all drives, but it calculates *two independent* parity blocks (often called "P" and "Q") for each stripe, using two different algorithms (one is XOR, the other is more complex). These two parity blocks are also rotated across different drives in the array.
* **Key Strength:** **Excellent fault tolerance.** It can survive *two* simultaneous drive failures, making it much safer for large arrays.
* **Key Weakness:** **Very slow write performance** (due to two complex parity calculations). Loses the capacity of two drives.
* **Parity:** Yes (Dual, e.g., XOR + Reed-Solomon).
* **Restoration:** Having two independent parity sets (`P` and `Q`) allows the controller to solve for *two* missing unknowns, restoring data even if two drives fail.

### RAID 10 (Stripe of Mirrors):
* **How it works:** This is a "nested" or "hybrid" level. First, you create two or more RAID 1 "mirror" sets (e.g., Drives 1&2 are a mirror, Drives 3&4 are a mirror). Then, you use RAID 0 to stripe data *across* those mirrored sets. So, Block 1 is written to the first pair, Block 2 to the second pair, etc.
* **Key Strength:** **The "best of both worlds."** It has the excellent performance of RAID 0 and the excellent redundancy of RAID 1. Rebuilds are very fast (just copying, no parity math).
* **Key Weakness:** **Very poor storage efficiency.** You lose 50% of your total capacity, just like RAID 1. It also requires at least 4 drives.
* **Parity:** None. Redundancy comes from the mirrors.
* **Redundancy:** Excellent. Can survive at least one drive failure (and potentially more, as long as no single mirrored pair loses both its drives).

---

### Comparison Table

| RAID Level | Minimum Drives | Capacity | Fault Tolerance | Read Speed | Write Speed |
| :--- | :---: | :--- | :--- | :--- | :--- |
| **RAID 0** | 2 | 100% | **None** | Fastest | Fastest |
| **RAID 1** | 2 | 50% | 1 Drive Failure | Fast | Slow |
| **RAID 5** | 3 | (N-1) * Size | 1 Drive Failure | Fast | Slow |
| **RAID 6** | 4 | (N-2) * Size | 2 Drive Failures | Fast | Very Slow |
| **RAID 10** | 4 | 50% | 1+ Drive Failure | Fastest | Fast |

---

### Dynamics: RAID, Storage Types, and Cloud

* **RAID as the Foundation:** The RAID principles (striping, mirroring, parity) are the fundamental building blocks for almost all reliable storage systems.
    * **Local Storage / DAS:** In a server or workstation, you use a **Hardware RAID controller** or **Software RAID** (in the OS) to protect data on its *internal* drives (e.g., RAID 1 for the OS, RAID 5/10 for data).
    * **NAS (Network Attached Storage):** A NAS is essentially a dedicated, small computer that uses **Software RAID** (like Linux's `mdadm` or ZFS) to manage its internal drives. It then *shares* that single, large, redundant storage volume with many users over the network (as file shares).
    * **SAN (Storage Area Network):** A SAN is a high-performance enterprise system that uses powerful **Hardware RAID** controllers to manage hundreds of drives. It presents this storage to servers over a high-speed network as *block* devices (which look like local drives), upon which servers might even run their *own* software RAID.

### **Cloud as an Abstraction and Evolution:** Cloud infrastructure takes these concepts and scales them up massively.
* **Cloud Emulates Traditional Storage:** Cloud providers offer services that directly mimic traditional storage types, but with the RAID and management *hidden* from you.
    * **Block Storage (e.g., AWS EBS, Azure Disk):** This is the cloud version of a **SAN**. You provision a "volume" (which is already redundant underneath) and attach it to a single virtual server. You can still apply your own *software RAID* on top of these volumes (e.g., RAID 0 two volumes for more speed).
    * **File Storage (e.g., AWS EFS, Azure Files):** This is the cloud's managed version of a **NAS**. It's a file share you can mount on many virtual servers at once.
* **Cloud Evolves RAID Principles:** For its massive-scale storage (like **Object Storage**, e.g., AWS S3), the cloud doesn't use "RAID 5." It uses the *principles* of RAID in a new way.
    * **Replication (like RAID 1):** Your data is fully copied (mirrored) to 3+ different physical servers in different data centers.
    * **Erasure Coding (like RAID 5/6):** Your data is split into data chunks and parity chunks, which are then striped across *dozens* of servers and racks. This allows the system to survive the failure of *entire servers or racks*—not just single disks—and is the direct evolution of the parity concept.

