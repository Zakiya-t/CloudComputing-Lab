# ⚡ Performance Analysis of Type-1 Hypervisor

## Proxmox VE Virtual Machine Performance Analysis

![Type-1 Hypervisor](https://img.shields.io/badge/Hypervisor-Type--1-blue)
![Proxmox VE](https://img.shields.io/badge/Proxmox%20VE-Type--1-E57000)
![Ubuntu](https://img.shields.io/badge/Guest%20OS-Ubuntu-orange)
![Sysbench](https://img.shields.io/badge/Benchmark-Sysbench-green)
![Status](https://img.shields.io/badge/Status-Experimental-success)

> **A practical performance-analysis experiment using Proxmox VE to study the behavior of an Ubuntu virtual machine running on a Type-1 hypervisor.**

---

## 📌 Overview

This project implements the **Type-1 Hypervisor performance-analysis experiment** using **Proxmox VE**.

The experiment involves creating an identically configured Ubuntu virtual machine on a Proxmox VE server, verifying its hardware and software configuration, monitoring resource utilization, and measuring CPU performance using **Sysbench**.

The objective is to establish a systematic and reproducible workflow for understanding how a virtual machine behaves when hosted on a **bare-metal Type-1 hypervisor**.

According to the laboratory manual, Proxmox VE is deployed on a centralized physical server and is accessed through its web-based management interface.

---

# 🎯 Objectives

The main objectives of this experiment are to:

* Understand the operation of a **Type-1 hypervisor** using Proxmox VE.
* Access and manage Proxmox VE through its web interface.
* Create and configure an Ubuntu virtual machine.
* Maintain fixed virtual hardware resources during the experiment.
* Verify CPU, memory, storage, network, and operating-system configuration.
* Monitor VM resource utilization.
* Measure CPU performance using Sysbench.
* Record benchmark observations for further comparison with a Type-2 hypervisor.
* Maintain raw experimental results and documentation for reproducibility.

---

# 🧠 What is a Type-1 Hypervisor?

A **Type-1 hypervisor**, also called a **bare-metal hypervisor**, runs directly on the physical server hardware rather than on top of a conventional host operating system.

In this experiment:

```text
Physical Server
       │
       ▼
┌──────────────────────┐
│    Proxmox VE        │
│   Type-1 Hypervisor  │
└──────────┬───────────┘
           │
           ▼
┌──────────────────────┐
│    Ubuntu VM         │
│                      │
│  2 vCPU              │
│  2 GB RAM            │
│  20 GB Disk          │
│  vmbr0 Network       │
└──────────────────────┘
```

The laboratory manual identifies **Proxmox VE as the Type-1 hypervisor** used in Part A of the experiment.

---

# 🏗️ Experimental Architecture

```text
                         PHYSICAL SERVER
                                │
                                ▼
                    ┌──────────────────────┐
                    │      Proxmox VE      │
                    │    Type-1 Hypervisor │
                    └──────────┬───────────┘
                               │
                               ▼
                    ┌──────────────────────┐
                    │      Ubuntu VM       │
                    ├──────────────────────┤
                    │ CPU      → 2 vCPU    │
                    │ Memory   → 2 GB      │
                    │ Disk     → 20 GB     │
                    │ Network  → vmbr0      │
                    └──────────┬───────────┘
                               │
             ┌─────────────────┼─────────────────┐
             ▼                 ▼                 ▼
       Verification       Monitoring        Benchmarking
             │                 │                 │
     ┌───────┴──────┐    ┌─────┴─────┐    ┌─────┴─────┐
     │ hostnamectl  │    │    top    │    │  Sysbench │
     │ lscpu        │    │           │    │    CPU    │
     │ free -h      │    │ CPU/RAM   │    │ Workload  │
     │ df -h        │    │ Load Avg  │    │           │
     └──────────────┘    └───────────┘    └─────┬─────┘
                                                │
                                                ▼
                                       Performance Results
                                                │
                                                ▼
                                      Type-1 Observation
```

---

# 🖥️ Experimental Environment

| Component              | Configuration                |
| ---------------------- | ---------------------------- |
| Hypervisor             | **Proxmox VE**               |
| Hypervisor Type        | **Type-1**                   |
| Guest Operating System | Ubuntu                       |
| Virtual CPU            | 2 vCPU                       |
| Memory                 | 2048 MiB / 2 GB              |
| Virtual Disk           | 20 GB                        |
| Storage                | local-lvm / assigned storage |
| Network Bridge         | `vmbr0`                      |
| Network Model          | Default / VirtIO             |
| CPU Configuration      | 1 socket × 2 cores           |
| Graphics               | Default                      |
| Machine                | Default                      |
| BIOS                   | Default                      |
| SCSI Controller        | Default                      |

## The VM configuration above follows the Type-1 configuration specified in the supplied laboratory manual.

# 🌐 Proxmox VE Web Interface

Proxmox VE is accessed through its web-based management interface.

The standard interface address used by the manual is:

```text
https://<PROXMOX_SERVER_IP>:8006
```

Example:

```text
https://192.168.X.X:8006
```

A browser certificate warning may appear because the server may use a self-signed SSL certificate.

The overall access workflow is:

```text
Network Connection
       ↓
Web Browser
       ↓
Proxmox VE Server
       ↓
Login Page
       ↓
Proxmox Dashboard
```

The Proxmox interface provides access to:

* Datacenter resources

* Proxmox nodes

* Virtual machines

* Storage

* Network configuration

* CPU and memory utilization

---

# 🛠️ Virtual Machine Creation

The Proxmox VM creation workflow used in the manual is:

```text
General
   ↓
OS
   ↓
System
   ↓
Disks
   ↓
CPU
   ↓
Memory
   ↓
Network
   ↓
Confirm
```

## VM Configuration

### General

| Parameter | Configuration          |
| --------- | ---------------------- |
| Node      | Selected Proxmox Node  |
| VM ID     | Automatically assigned |
| Name      | `<Name>-Type1`         |

Example:

```text
CC-Experiment1-Type1
```

### Operating System

```text
Installation Media → ISO Image
Storage            → local
Operating System   → Ubuntu
```

### Disk

```text
Storage     → local-lvm / assigned storage
Disk Size   → 20 GB
Bus/Device  → Default
```

### CPU

```text
Sockets = 1
Cores   = 2
----------------
Total   = 2 vCPU
```

### Memory

```text
Memory = 2048 MiB
       = 2 GB
```

### Network

```text
Bridge = vmbr0
Model  = Default / VirtIO
```

## These settings are directly based on the Type-1 VM configuration in the manual.

# ✅ VM Verification

After the Ubuntu VM is created and started, the following information is verified.

## Operating System and Kernel

```bash
hostnamectl
```

The following are checked:

```text
Hostname
Operating System
Kernel Version
Architecture
```

## CPU

```bash
lscpu
```

Important parameters:

```text
Architecture
CPU(s)
CPU Model
Number of Cores
Virtualization Information
```

## Memory

```bash
free -h
```

Observed parameters:

```text
Total Memory
Used Memory
Free Memory
Available Memory
```

## Disk

```bash
df -h
```

Observed parameters:

```text
Filesystem
Total Capacity
Used Space
Available Space
```

The manual specifies these verification commands before proceeding with the performance analysis.

---

# 📊 Resource Monitoring

System resources are monitored from inside the Ubuntu VM using:

```bash
top
```

The experiment observes:

```text
CPU Utilization
Memory Utilization
Running Processes
Load Average
```

To exit:

```text
q
```

Proxmox VE also provides VM-level monitoring through:

```text
Datacenter
   ↓
Proxmox Node
   ↓
Virtual Machine
   ↓
Summary
```

Important resource observations include:

```text
CPU Usage
Memory Usage
Network Traffic
Disk Usage
```

---

# ⚙️ CPU Performance Benchmark

CPU performance is evaluated using **Sysbench**.

## Tool Installation

```bash
sudo apt update
sudo apt install sysbench -y
```

Verify:

```bash
sysbench --version
```

## Benchmark Workload

```bash
sysbench cpu --cpu-max-prime=20000 run
```

The following parameters are recorded:

```text
Total Execution Time
Total Number of Events
Events per Second
Minimum Latency
Average Latency
Maximum Latency
```

The Type-1 manual specifies Sysbench CPU prime calculation with a maximum prime value of `20000`.

---

# 📈 Type-1 CPU Results

> **Replace the placeholder links below with the exact result files from the `results/` directory.**

### Raw Benchmark Output

[View Type-1 CPU Raw Result](results/type1/raw/cpu.txt)

### Processed Result

[View Type-1 CPU Processed Result](results/type1/processed/cpu_results.csv)

### Result Summary

| Metric               |    Type-1 Result |
| -------------------- | ---------------: |
| Hypervisor           |       Proxmox VE |
| Hypervisor Type      |           Type-1 |
| Guest OS             |           Ubuntu |
| CPU                  |           2 vCPU |
| Memory               |             2 GB |
| Disk                 |            20 GB |
| Total Execution Time | **[ADD RESULT]** |
| Total Events         | **[ADD RESULT]** |
| Events/sec           | **[ADD RESULT]** |
| Minimum Latency      | **[ADD RESULT]** |
| Average Latency      | **[ADD RESULT]** |
| Maximum Latency      | **[ADD RESULT]** |

---

# 📊 Performance Visualization

Add the generated Type-1 benchmark graph here:

```markdown
![Type-1 CPU Performance](results/type1/figures/cpu_performance.png)
```

Or link directly to the figure:

[📈 View Type-1 CPU Performance Graph](results/type1/figures/cpu_performance.png)

> Replace the path above with the actual figure path present in your repository.

---

# 🔬 Observation Table

The manual provides an observation table for the Type-1 hypervisor.

| Parameter              | Observation      |
| ---------------------- | ---------------- |
| Hypervisor             | Proxmox VE       |
| Hypervisor Type        | Type-1           |
| Guest Operating System | Ubuntu           |
| CPU Allocation         | 2 vCPU           |
| Memory Allocation      | 2 GB             |
| Disk Allocation        | 20 GB            |
| Total Execution Time   | **[ADD RESULT]** |
| Total Events           | **[ADD RESULT]** |
| Events per Second      | **[ADD RESULT]** |
| Average Latency        | **[ADD RESULT]** |

This structure follows the Type-1 observation section of the supplied manual.

---

# 📁 Results Organization

The experimental results are maintained separately from the source documentation.

Recommended structure:

```text
results/
│
├── type1/
│   ├── raw/
│   │   └── cpu.txt
│   │
│   ├── processed/
│   │   └── cpu_results.csv
│   │
│   └── figures/
│       └── cpu_performance.png
│
└── ...
```

> Update these paths to match the actual structure of your repository.

---

# 🔁 Experimental Workflow

The complete Type-1 workflow can be summarized as:

```text
Start
  │
  ▼
Access Proxmox VE
  │
  ▼
Login to Proxmox
  │
  ▼
Select Proxmox Node
  │
  ▼
Create VM
  │
  ▼
Configure Ubuntu
  │
  ▼
Configure 2 vCPU
  │
  ▼
Configure 2 GB RAM
  │
  ▼
Configure 20 GB Disk
  │
  ▼
Configure vmbr0 Network
  │
  ▼
Start VM
  │
  ▼
Install Ubuntu
  │
  ▼
Verify Configuration
  │
  ├── hostnamectl
  ├── lscpu
  ├── free -h
  └── df -h
  │
  ▼
Monitor Resources
  │
  ▼
Install Sysbench
  │
  ▼
Run CPU Benchmark
  │
  ▼
Record Results
  │
  ▼
Store Results
  │
  ▼
Analysis / Comparison
  │
  ▼
Shutdown VM
```

---

# 🧪 Reproducibility

To reproduce the experiment:

### 1. Access Proxmox VE

```text
https://<PROXMOX_SERVER_IP>:8006
```

### 2. Create the VM

Configure:

```text
CPU       → 1 socket × 2 cores
Memory    → 2048 MiB
Disk      → 20 GB
Network   → vmbr0
Guest OS  → Ubuntu
```

### 3. Verify the VM

```bash
hostnamectl
lscpu
free -h
df -h
```

### 4. Install Sysbench

```bash
sudo apt update
sudo apt install sysbench -y
```

### 5. Run the CPU Benchmark

```bash
sysbench cpu --cpu-max-prime=20000 run
```

### 6. Record the Output

Record:

```text
Execution Time
Total Events
Events/sec
Minimum Latency
Average Latency
Maximum Latency
```

### 7. Monitor the VM

```bash
top
```

Proxmox resource monitoring can also be observed from the VM's Summary page.

### 8. Store the Results

Keep benchmark output inside the repository's `results/` directory.

---

# 📝 Key Observations

The experiment is designed to study the behavior of an Ubuntu VM running under a Type-1 hypervisor while keeping virtual hardware configuration fixed.

Important observations include:

* Virtual CPU allocation.
* Guest memory allocation.
* Virtual storage configuration.
* Network bridge configuration.
* CPU utilization during benchmarking.
* Memory utilization during execution.
* Sysbench CPU throughput.
* CPU latency statistics.
* VM-level resource activity visible through Proxmox VE.

The numerical performance conclusion should be based on the actual collected benchmark measurements rather than assumed behavior.

---

# 🆚 Relation to Type-2 Hypervisor Analysis

This Type-1 experiment forms **Part A** of the laboratory exercise.

The manual's second part uses:

```text
Proxmox VE       → Type-1
VMware Workstation → Type-2
```

The purpose of maintaining the same VM resource configuration is to enable a later comparison between the two hypervisor types.

The Type-2 section of the manual uses the same basic guest resource configuration:

```text
2 vCPU
2 GB RAM
20 GB Disk
Ubuntu
```

## This makes the Type-1 measurements a foundation for the subsequent Type-2 comparison.

# 📂 Project Structure

```text
type1-hypervisor-performance/
│
├── README.md
│
├── docs/
│   ├── architecture.png
│   ├── vm-configuration.txt
│   └── ...
│
├── scripts/
│   └── ...
│
├── results/
│   ├── raw/
│   ├── processed/
│   └── figures/
│
└── ...
```

> Update the structure above to exactly match the final repository folders.

---

# 📚 Technologies Used

| Technology          | Purpose                                |
| ------------------- | -------------------------------------- |
| **Proxmox VE**      | Type-1 hypervisor                      |
| **Ubuntu**          | Guest operating system                 |
| **Sysbench**        | CPU performance benchmarking           |
| **top**             | System resource monitoring             |
| **Linux utilities** | CPU, memory, disk, and OS verification |

---

# ✅ Experiment Checklist

```text
☐ Access Proxmox VE
☐ Log in to Proxmox
☐ Create Ubuntu VM
☐ Configure 2 vCPU
☐ Configure 2 GB RAM
☐ Configure 20 GB disk
☐ Configure vmbr0
☐ Start VM
☐ Install Ubuntu
☐ Verify hostname and OS
☐ Verify CPU configuration
☐ Verify memory configuration
☐ Verify disk configuration
☐ Monitor CPU and memory
☐ Install Sysbench
☐ Run CPU benchmark
☐ Record benchmark metrics
☐ Save raw results
☐ Save processed results
☐ Add performance graph
☐ Complete Type-1 analysis
☐ Shutdown VM
```

---

# 📖 Reference

**Performance Analysis of Type-1 and Type-2 Hypervisors — Lab Manual**

Part A: **Performance Analysis Using Type-1 Hypervisor – Proxmox VE**

The procedure, VM configuration, verification steps, monitoring workflow, and CPU benchmark methodology documented here are based on the supplied laboratory manual.

---

## 👩‍💻 Project

**Performance Analysis of Type-1 and Type-2 Hypervisors**

**Type-1 Platform:** Proxmox VE
**Guest OS:** Ubuntu
**Benchmark:** Sysbench CPU Performance

