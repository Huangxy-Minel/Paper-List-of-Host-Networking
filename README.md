# Reading-List-of-Host-Networking
## Introduction
The performance of data center network facilities, including switches, routers, and network interface cards (NICs), has advanced significantly in recent years: the network bandwidth has surged from 10 Gbps to 100/200/400/800 Gbps to accommodate the increasing communication demands of datacenter applications such as cloud computing, big data analytics, machine learning and AI. Although promising, the performance of network Input/Output (I/O) architectures remains suboptimal due to inefficient utilization of host resources, limiting the full potential of data center network infrastructure. We refer the network consists of intra-host endpoints including CPUs, GPUs, NICs, interconnections (e.g., PCIe and NV-switch), and disks to **Host Network**. In this topic, we aim to explore more efficient I/O data path (responsible for data movement) and control path (responsible for routing, scheduling, traffic controlling, etc.) to improve the end-to-end throughput and reduce tail latency between every two endpoints.

This reading list is mainly served for new students of iSING Lab @ HKUST. So far (02/12/2024), most of works mainly focus on the path between CPUs and NICs. Thus, the story for this reading list is about where the bottleneck occurs and how to address it in the CPU-to-NIC path. We believe there will be more works target to other paths like GPU-to-GPU, CPU-to-GPU, GPU-to-NIC or target to the utilization of interconnection. Hopefully the list can help readers to explore interesting and exciting ideas.

**Contribute the reading list**: write an issue to recommend a work you find interesting. Please include the source and a brief description of the job.

## Selected readings
It is recommended in the order of the list.

### 1. Basic
- 16-ATC-Design Guidelines for High Performance RDMA Systems
- 18-SIGCOMM-Revisiting Network Support for RDMA
- 19-NSDI-Datacenter RPCs can be General and Fast
- 20-OSDI-A Simpler and Faster NIC Driver Model for Network Functions
- 20-FCCM-Corundum-An Open-Source 100-Gbps NIC
- 21-SIGCOMM-Understanding host network stack overheads

### 2. Computation Offloading
**[Network Functions]**
- 20-OSDI-PANIC: A High-Performance Programmable NIC for Multi-tenant Networks
- 20-OSDI-hXDP: Efficient Software Packet Processing on FPGA NICs
- 22-NSDI-Tiara: A Scalable and Efficient Hardware Acceleration Architecture for Stateful Layer-4 Load Balancing

**[Network Stack]**
- 20-NSDI-Enabling programmable transport protocols in high-speed NIC
- 23-NSDI-SRNIC: A Scalable Architecture for RDMA NICs
- 24-OSDI-High-throughput and Flexible Host Networking for Accelerated Computing

**[Multi-Tenant]**
- 19-SIGCOMM-Offloading distributed applications onto smartNICs using iPipe
- 24-FPGA-SuperNIC: An FPGA-Based, Cloud-Oriented SmartNIC

### 3. Shorten or Simplify
**[Memory to Memory]**
- 14-NSDI-FaRM: Fast Remote Memory

**[CPUs to CPUs]**
- 16-OSDI-FaSST: Fast, Scalable and Simple Distributed Transactions with Two-Sided (RDMA) Datagram RPCs
- 18-CoNEXT-The eXpress Data Path: Fast Programmable Packet Processing in the Operating System Kernel

**[Accelerators to Accelerators]**
- 22-ATC-FpgaNIC: An FPGA-based Versatile 100Gb SmartNIC for GPUs


### 4. Profiling
- 18-SIGCOMM-Understanding PCIe performance for end host networking
- 20-ATC-Reexamining Direct Cache Access to Optimize IO Intensive Applications for Multi-hundred-gigabit Networks
- 24-SIGCOMM-Understanding the Host Network

### 5. Resource Management
**[CPU Cores]**
- 14-NSDI-mTCP-a Highly Scalable User-level TCP Stack for Multicore Systems
- 22-SIGCOMM-Towards us Tail Latency and Terabit Ethernet - Disaggregating the Host Network Stack

**[Memory Efficiency]**
- 23-SIGCOMM-Host Congestion Control
- 23-OSDI-Ensō: A Streaming Interface for NIC-Application Communication
- 23-OSDI-ShRing: Networking with Shared Receive Rings

**[Datapath OS]**
- 14-OSDI-IX: A Protected Dataplane Operating System for High Throughput and Low Latency
- 21-SOSP-The Demikernel Datapath OS Architecture for Microsecond-scale Datacenter Systems
- 24-NSDI-Making Kernel Bypass Practical for the Cloud with Junction

