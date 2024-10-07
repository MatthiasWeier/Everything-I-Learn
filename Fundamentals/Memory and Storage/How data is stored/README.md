# How Data is Stored

In modern computing, data storage and retrieval are fundamental processes that occur both at the hardware and software levels. Understanding how data is stored involves looking at how memory addresses operate in hardware, as well as how file systems structure and manage data on storage devices like hard drives and SSDs. This page will break down these concepts, covering memory addresses, data retrieval in hardware, and an introduction to popular file systems such as FAT32, NTFS, and ext4.

## Memory Addresses and Data Retrieval in Hardware

In the realm of hardware, data is stored and accessed using **memory addresses**. A memory address is a unique identifier that corresponds to a specific location in the computer’s memory, whether it be RAM (Random Access Memory) or other types of memory.

### 1. **Memory Addresses**
   
   Each byte in memory is assigned an address, which the CPU uses to locate and retrieve data. The memory address is typically represented as a **hexadecimal number**. For example:
   
   ```
   0x0001, 0x0002, 0x0003, ...
   ```

   "0x" is a prefix for hexadecimal.
   These addresses form a linear structure known as an **address space**, which can be accessed by the system's hardware.

   ### Memory Hierarchy and Types of Memory
   Modern systems employ a memory hierarchy to balance speed and cost. Different types of memory store data, with **cache memory** being the fastest and most expensive, followed by **RAM**, and then **secondary storage** (like SSDs and HDDs), which is slower but cheaper.

   ```mermaid
   graph TD;
       A[Cache Memory] --> B[RAM];
       B --> C[SSD/HDD];
       style A fill:#f9f,stroke:#333,stroke-width:4px;
   ```

   #### Key Memory Types:
   - **RAM** (Volatile Memory): Temporary storage used to hold data that is being actively worked on. Once the computer is powered off, all data in RAM is lost.
   - **ROM** (Non-volatile Memory): Stores critical information such as the BIOS. Data remains even when the computer is powered off.
   - **Secondary Storage**: Non-volatile storage like SSDs or HDDs that permanently store data.

### 2. **Data Retrieval Process**

   Data retrieval in hardware follows a process where the CPU interacts with the memory controller to locate specific memory addresses. The steps include:
   - **Addressing**: The CPU sends the memory address it wants to access.
   - **Fetching**: The memory controller retrieves the data from the specified address.
   - **Data Transfer**: The data is then sent back to the CPU for processing.

   This process happens incredibly fast, typically in nanoseconds, ensuring the CPU has the necessary data for computations. More to this on my Hardware/CPU page.

## Introduction to File Systems

File systems are the methods and structures an operating system uses to organize, store, retrieve, and manage files on storage devices such as hard drives, solid-state drives, and removable media. Each file system has its own method of handling **metadata**, **file allocation**, and **storage space**.

### 1. **FAT32 (File Allocation Table 32)**

   FAT32 is an older but widely used file system, especially in portable storage devices like USB drives.

   - **Developed By**: Microsoft
   - **Advantages**:
     - Supported by almost all operating systems.
     - Simple structure, making it suitable for flash drives.
   - **Limitations**:
     - File size limit of 4GB.
     - Volume size limit of 32GB (in most OS implementations).
   
   FAT32 organizes data into clusters and maintains a **File Allocation Table**, which acts as an index to locate files on the disk. When data is stored, the FAT table tracks where each chunk of a file resides.

   ```mermaid
   graph LR;
   A[File Allocation Table] --> B[Data Cluster 1];
   A --> C[Data Cluster 2];
   A --> D[Data Cluster 3];
   ```

### 2. **NTFS (New Technology File System)**

   NTFS is a modern file system used primarily by Windows operating systems.

   - **Developed By**: Microsoft
   - **Advantages**:
     - Supports large files and large volumes (up to several terabytes).
     - Features security, such as file permissions and encryption (e.g., BitLocker).
     - Journaled file system, improving data integrity in case of crashes.
   - **Limitations**:
     - Limited support outside Windows without additional software.
   
   NTFS uses a **Master File Table (MFT)** to store information about every file and directory, allowing for efficient management and retrieval of data.

### 3. **ext4 (Fourth Extended File System)**

   ext4 is a widely used file system in Linux-based operating systems.

   - **Developed By**: Linux community
   - **Advantages**:
     - Supports large volumes and files (up to 1 exabyte).
     - Offers journaling for improved reliability.
     - Extends support for timestamps and improved defragmentation.
   - **Limitations**:
     - Limited support on non-Linux platforms.
   
   ext4 improves upon its predecessors (ext2 and ext3) by offering better performance and scalability. It uses **extent-based storage**, which means contiguous blocks of data are stored together, reducing fragmentation and improving read/write speeds.

   ```mermaid
   graph TD;
       A[File] --> B[Extent 1: Blocks 0-9];
       A --> C[Extent 2: Blocks 10-19];
   ```

There are so many more filesystem. I made this big table to cover most of them. I don't go in depth into everything. Currently these are the most used (lets not get into apple in depth).
## Filesystems Guide (Windows, Linux, macOS)

| Filesystem | OS Compatibility | Max File Size | Max Volume Size | Purpose/Usage                     | Key Features                                                                                   |
|------------|------------------|---------------|-----------------|-----------------------------------|------------------------------------------------------------------------------------------------|
| **FAT12**  | Windows, DOS, Linux | 32MB          | 16MB - 32MB      | Older storage media (e.g., floppy disks) | Simple, outdated, limited by small storage size                                                |
| **FAT16**  | Windows, DOS, Linux | 2GB - 4GB     | 2GB - 16GB       | Older devices, smaller flash drives | Simple, wider compatibility, but limited by size                                               |
| **FAT32**  | Windows, macOS, Linux | 4GB           | 2TB (up to 16TB) | Flash drives, external hard disks   | Good compatibility, supports larger sizes than FAT16                                           |
| **exFAT**  | Windows XP+, macOS, Linux | 16EB          | 128PB            | Flash drives, SD cards, external HDDs | Large file support, modern replacement for FAT32                                               |
| **NTFS**   | Windows, macOS (read-only), Linux (NTFS-3G for write support) | 16EB          | 16EB              | Windows systems, external drives    | Security features, file compression, journaling, supports large files                           |
| **ReiserFS** | Linux            | 8TB           | 16TB             | Linux systems                      | Efficient for small files, journaling                                                          |
| **ReFS**   | Windows Server 2012+, Windows Pro for Workstations | 16EB          | 1YB              | Enterprise, business, data centers | Resilience, large data handling, auto-correcting capability                                    |
| **ext2**   | Linux, macOS (read-only), Windows (read-only) | 2TB - 16TB    | 2TB - 32TB        | Linux systems, older installations  | Lightweight, no journaling, fast for smaller systems                                           |
| **ext3**   | Linux, macOS (read-only), Windows (read-only) | 2TB - 16TB    | 2TB - 32TB        | Linux systems                      | Journaling, backward compatibility with ext2                                                   |
| **ext4**   | Linux, macOS (read-only), Windows (read-only) | 16TB          | 1EB              | Linux systems                      | Journaling, efficient allocation, large volume and file size support                           |
| **btrfs**  | Linux, macOS (read-only) | 16EB          | 16EB              | Linux systems, NAS                 | Copy-on-write, snapshots, data integrity                                                       |
| **Minix**  | Minix, Linux (limited) | 64MB - 4GB    | 64MB - 4GB        | Minix OS, education                 | Simple, designed for older or educational purposes                                             |
| **UDF**    | Windows, macOS, Linux | 16EB          | 128TB            | Optical media (DVDs, USB drives)   | Interoperability across different systems, designed for optical media                          |
| **HFS**    | macOS, Windows (read-only), Linux | 2GB           | 2TB              | Older macOS systems                | Mac-specific, metadata, and resource forks                                                     |
| **HFS+**   | macOS, Windows (read-only), Linux | 8EB           | 8EB              | macOS systems                      | Improved version of HFS, supports larger sizes and Unicode                                     |
| **APFS**   | macOS, iOS, tvOS, watchOS | 8EB           | 8EB              | Apple devices                      | Encryption, space sharing, snapshots, fast directory sizing                                    |
| **ZFS**    | FreeBSD, Linux, Solaris (limited support in other OS) | 16EB          | 256ZB            | Servers, NAS, high-end workstations | Data integrity, volume management, snapshots, replication                                      |
| **Stratis**| Linux             | 16EB          | 16EB             | Linux systems                      | Easy volume management, snapshots, thin provisioning                                            |
| **bcachefs**| Linux             | 16EB          | 16EB             | Linux systems                      | Copy-on-write (COW), caching, snapshots, checksums, compression, encryption                    |

### Standard Filesystem Usage Across OS:
- **Windows Standard**: NTFS
- **Linux Standard**: ext4, btrfs, ZFS, Stratis
- **macOS Standard**: APFS, HFS+

