# System Design for Host Networking
## Introduction
The performance of data center network facilities, including switches, routers, and network interface cards (NICs), has advanced significantly in recent years: the network bandwidth has surged from 10 Gbps to 100/200/400/800 Gbps to accommodate the increasing communication demands of datacenter applications such as cloud computing, big data analytics, machine learning and AI. Although promising, the performance of network Input/Output (I/O) architectures remains suboptimal due to inefficient utilization of host resources, limiting the full potential of data center network infrastructure. We refer the network consists of intra-host endpoints including CPUs, GPUs, NICs, interconnections (e.g., PCIe and NV-switch), and disks to **Host Network**. In this topic, we aim to explore more efficient I/O data path (responsible for data movement) and control path (responsible for routing, scheduling, traffic controlling, etc.) to improve the end-to-end throughput and reduce tail latency between every two endpoints.

This reading list is mainly served for new students of [iSING Lab @ HKUST](https://ising.cse.ust.hk/). So far (02/01/2025), most of works mainly focus on the path between CPUs and NICs. Thus, the story for this reading list is about where the bottleneck occurs and how to address it in the CPU-to-NIC path. We believe there will be more works target to other paths like GPU-to-GPU, CPU-to-GPU, GPU-to-NIC or target to the utilization of interconnection. Hopefully the list can help readers to explore interesting and exciting ideas.

**Contribute the reading list**: write an issue to recommend a work you find interesting. Please include the source and a brief description of the job.

## Catalog
The page is basically divided into two parts: (a) a selected readings to construct the foundation of host networking research; and (b) paper list that categorized based on research topics.
#### [1 Selected Readings](#1)
- [1.1 Basic](#1.1)
- [1.2 Computation Offloading](#1.2)
- [1.3 Shorten or Simplify Data Path](#1.3)
- [1.4 Profiling](#1.4)
- [1.5 Resource Management](#1.5)

#### [2 Research Topics](#2)
- [2.1 Network Subsystem Architecture](#2.1)
  - [Sub-Topic 1: Network Stack Design and Architecture](#2.1.1)
  - [Sub-Topic 2: Network Function (NF) Chain](#2.1.2)
  - [Sub-Topic 3: Domain-Specific Network Architecture](#2.1.3)
  - [Sub-Topic 4: CPU-Centric Dataplane OSes](#2.1.4)
- [2.2 SmartNIC Infrastructure](#2.2)
  - [Sub-Topic 1: Programmabiliby](#2.2.1)
  - [Sub-Topic 2: Offloading Frameworks](#2.2.2)
  - [Sub-Topic 3: Virtualization and Isolation](#2.2.3)
 
#### [3 Open-Source Projects](#3)

#### [4 Researchers](#4)

<h2 id="1">1 Selected Readings</h2>
It is recommended in the order of the list.

<h3 id="1.1">1.1 Basic</h3>
- 16-ATC-Design Guidelines for High Performance RDMA Systems
- 18-SIGCOMM-Revisiting Network Support for RDMA
- 19-NSDI-Datacenter RPCs can be General and Fast
- 20-OSDI-A Simpler and Faster NIC Driver Model for Network Functions
- 20-FCCM-Corundum-An Open-Source 100-Gbps NIC
- 21-SIGCOMM-Understanding host network stack overheads

<h3 id="1.2">1.2 Computation Offloading</h3>

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

<h3 id="1.3">1.3 Shorten or Simplify Data Path</h3>

**[Memory to Memory]**
- 14-NSDI-FaRM: Fast Remote Memory

**[CPUs to CPUs]**
- 16-OSDI-FaSST: Fast, Scalable and Simple Distributed Transactions with Two-Sided (RDMA) Datagram RPCs
- 18-CoNEXT-The eXpress Data Path: Fast Programmable Packet Processing in the Operating System Kernel

**[Accelerators to Accelerators]**
- 22-ATC-FpgaNIC: An FPGA-based Versatile 100Gb SmartNIC for GPUs

<h3 id="1.4">1.4 Profiling</h3>
- 18-SIGCOMM-Understanding PCIe performance for end host networking
- 20-ATC-Reexamining Direct Cache Access to Optimize IO Intensive Applications for Multi-hundred-gigabit Networks
- 24-SIGCOMM-Understanding the Host Network

<h3 id="1.5">1.5 Resource Management</h3>

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

---

<h2 id="2">2 Reseach Topics in HostNet</h2>
This section serves as a record of the exploration and previous topics in HostNet @ iSING. Each section will highlight core questions that our community aims to address. By analyzing this page, we aim to derive the emerging research trends in network systems, starting from the foundational TCP/IP architecture with socket programming in Linux operating systems.

**Contribute to this section**: Please write an issue to summarize the topic, a short description, questions, and related works that you think interesting!

<h3 id="2.1">2.1 Network Subsystem Architecture</h3>

The TCP/IP architecture, which primarily relies on the Linux kernel to manage and process the network subsystem, often leads to CPU overload. In response, our community is exploring a re-design of the network subsystem architecture, shifting from kernel-centric and CPU-centric models to disaggregated approaches. This includes the introduction of kernel-bypass architectures, NIC offloads, RDMA, and other innovations.

<h4 id="2.1.1">Sub-Topic 1: Network Stack Design and Architecture</h4>

**[Questions]**
- How can we simplify or accelerate network stack processing to alleviate the CPU burden?
- Can we design a high-performance, highly flexible network stack architecture?

**[Related Works]**
- **18-SIGCOMM-Revisiting Network Support for RDMA**
- 20-NSDI-AccelTCP-Accelerating Network Applications with Stateful TCP Offloading
- 20-SIGCOMM-1RMA-Re-envisioning Remote Memory Access for Multi-tenant Datacenters
- 21-SOSR-NanoTransport: A Low-Latency, Programmable Transport Layer for NICs
- 21-OSDI-The nanoPU-A Nanosecond Network Stack for Datacenters
- 22-NSDI-FlexTOE- Flexible TCP Offload with Fine-Grained Parallelism
- 23-NSDI-Rearchitecting the TCP Stack for IO-Offloaded Content Delivery
- **23-NSDI-SRNIC- A Scalable Architecture for RDMA NICs**
- **24-OSDI-High-throughput and Flexible Host Networking for Accelerated Computing**

<h4 id="2.1.2">Sub-Topic 2: Network Function (NF) Chain</h4>

**[Questions]**
- How can we achieve line-rate throughput for a given NF chain?
- Can we design a programming and execution framework NFs?

**[Related Works]**
- 14-SIGCOMM-OpenNF-Enabling Innovation in Network Function Control
- **15-NSDI-The Design and Implementation of Open vSwitch**
- 17-SIGCOMM-NFP-Enabling Network Function Parallelism in NFV
- 17-SIGCOMM-NFVnice-Dynamic Backpressure and Scheduling for NFV Service Chains
- 18-NSDI-Azure Accelerated Networking-SmartNICs in the Public Cloud
- **18-CoNEXT-The eXpress Data Path-Fast Programmable Packet Processing in the Operating System Kernel**
- 20-OSDI-A Simpler and Faster NIC Driver Model for Network Functions
- 22-ASPLOS-The Benefits of General-Purpose On-NIC Memory
- **22-NSDI-Tiara-A Scalable and Efficient Hardware Acceleration Architecture for Stateful Layer-4 Load Balancing**
- 23-NSDI-Disaggregating Stateful Network Functions
- 24-EuroSys-Hoda-a High-performance Open vSwitch Dataplane with Multiple Specialized Data Paths
- 24-ATC-CyberStar-Simple, Elastic and Cost-Effective Network Functions Management in Cloud Network at Scale

<h4 id="2.1.3">Sub-Topic 3: Domain-Specific Network Architecture</h4>

**[Questions]**
- How can we achieve 100Gbps or higher network transmission for a specific application?
- Can we accelerate network-intensive applications using RDMA NICs or SmartNICs?

**[Related Works]**
- **14-NSDI-FaRM-Fast Remote Memory**
- 14-SIGCOMM-Using RDMA Efficiently for Key-Value Services
- **16-OSDI-FaSST-Fast, Scalable and Simple Distributed Transactions with Two-Sided (RDMA) Datagram RPCs**
- 17-SOSP-KV-Direct-High-Performance In-Memory Key-Value Store with Programmable NIC
- 18-OSDI-Deconstructing RDMA-enabled Distributed Transactions- Hybrid is Better
- **19-NSDI-Datacenter RPCs can be General and Fast**
- 19-SIGCOMM-SocksDirect-Datacenter sockets can be fast and compatible
- 20-NSDI-TCP≈RDMA-CPU-efficient Remote Storage Access with i10
- 20-OSDI-Fast RDMA-based Ordered Key-Value Store using Remote Learned Cache
- 20-ISCA-The NEBULA RPC-Optimized Architecture
- 21-SOSP-LineFS-Efficient SmartNIC Offload of a Distributed File System with Pipeline Parallelism
- 21-SOSP-Xenic-Smartnic-accelerated distributed transactions
- 21-SOSP-PRISM-Rethinking the RDMA Interface for Distributed Systems
- 21-NSDI-BMC-Accelerating Memcached using Safe In-kernel Caching and Pre-stack Processing
- 22-OSDI-XRP-In-Kernel Storage Functions with eBPF
- 22-NSDI-RDMA is Turing complete we just did not know it yet
- **23-NSDI-Empowering Azure Storage with RDMA**
- 24-EuroSys-Serialization-Deserialization-free State Transfer in Serverless Workflows

<h4 id="2.1.4">Sub-Topic 4: CPU-Centric Dataplane OSes</h4>

**[Questions]**
- How to design a general-proposed dataplane OS with high-performance for various applications?
- How to schedule CPU cores for high-efficiency?

**[Related Works]**
- **14-NSDI-mTCP-a Highly Scalable User-level TCP Stack for Multicore Systems**
- 14-OSDI-IX-A Protected Dataplane Operating System for High Throughput and Low Latency
- 17-SOSP-ZygOS-Achieving Low Tail Latency for Microsecond-scale Networked Tasks
- 19-NSDI-Shenango-Achieving High CPU Efficiency for Latency-sensitive Datacenter Workloads
- 19-NSDI-Shinjuku-Preemptive Scheduling for µsecond-scale Tail Latency
- 20-OSDI-Caladan-Mitigating Interference at Microsecond Timescales
- 21-SOSP-The Demikernel Datapath OS Architecture for Microsecond-scale Datacenter Systems
- 22-ATC-AlNiCo-SmartNIC-accelerated Contention-aware Request Scheduling for Transaction Processing
- **22-SIGCOMM-Towards us Tail Latency and Terabit Ethernet- Disaggregating the Host Network Stack**
- 23-NSDI-RingLeader-Efficiently Offloading Intra-Server Orchestration to NICs
- **24-NSDI-Making Kernel Bypass Practical for the Cloud with Junction**

---

<h3 id="2.2">2.2 SmartNIC Infrastructure</h3>

Given the growing reliance on SmartNICs for processing network workloads at the end host, it is imperative to update the SmartNIC infrastructure. Below, we outline several new features that our community aims to develop or optimize. This includes programmablity, offloading frameworks, and virtualization/isolation.

<h4 id="2.2.1">Sub-Topic 1: Programmabiliby</h4>

**[Questions]**
- How can we enable programmability in SmartNICs without compromising high performance?

**[Related Works]**
- 19-NSDI-FlowBlaze-Stateful Packet Processing in Hardware
- 20-NSDI-Enabling programmable transport protocols in high-speed NIC
- 20-OSDI-hXDP-Efficient Software Packet Processing on FPGA NICs
- 22-ATC-Faster Software Packet Processing on FPGA NICs with eBPF Program Warping

<h4 id="2.2.2">Sub-Topic 2: Offloading Frameworks</h4>

**[Questions]**
- Can we design offloading frameworks that automatically optimize and maximize SmartNIC performance?

**[Related Works]**
- 18-OSDI-Floem-A Programming System for NIC-Accelerated Network Applications
- 19-SIGCOMM-Offloading distributed applications onto smartNICs using iPipe
- 21-ASPLOS-Autonomous NIC Offloads

<h4 id="2.2.3">Sub-Topic 3: Virtualization and Isolation</h4>

**[Questions]**
- How can we implement virtualization and isolation features for NICs in cloud environments?

**[Related Works]**
- 20-SIGCOMM-SmartNIC Performance Isolation with FairNIC-Programmable Networking for the Cloud
- 22-NSDI-Collie-Finding Performance Anomalies in RDMA Subsystems
- 22-NSDI-Isolation Mechanisms for High-Speed Packet-Processing Pipelines
- 24-EuroSys-HD-IOV: SW-HW Co-designed I/O Virtualization with Scalability and Flexibility for Hyper-Density Cloud
- 24-EuroSys-SmartNIC Security Isolation in the Cloud with S-NIC

<h2 id="3">3 Open-Source Projects</h2>
- [DPDK](https://www.dpdk.org/): The Open Source Data Plane Development Kit Accelerating Network Performance
- [rdma-core](https://github.com/linux-rdma/rdma-core): RDMA core userspace libraries and daemons
- [open-rdma](https://github.com/datenlord/open-rdma): RoCE v2 hardware and software implementation
- [XDP](https://github.com/xdp-project/xdp-tutorial): eXpress Data Path (XDP) system
- [OvS](https://github.com/openvswitch/ovs): Production Quality, Multilayer Open Virtual Switch
- [Katran](https://github.com/facebookincubator/katran): A high performance layer 4 load balancer
- [eRPC](https://github.com/erpc-io/eRPC): Efficient RPCs for datacenter networks
- [Junction](https://github.com/JunctionOS/junction): Next-generation datacenter OS built on kernel bypass to speed up unmodified code while improving platform density and security
- [corundum](https://github.com/corundum/corundum): Open source FPGA-based NIC and platform for in-network compute
- [DOCA](https://developer.nvidia.com/networking/doca): Accelerate application development for NVIDIA BlueField and ConnectX networking devices

<h2 id="4">4 Researchers</h2>
