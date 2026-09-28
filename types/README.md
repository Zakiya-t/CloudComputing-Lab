# ⚡ Performance Analysis of Type-1 and Type-2 Hypervisors

### Proxmox VE 🖥️ vs VMware Workstation 💻

![Type-1](https://img.shields.io/badge/Type--1-Proxmox%20VE-E57000?style=for-the-badge)
![Type-2](https://img.shields.io/badge/Type--2-VMware%20Workstation-607078?style=for-the-badge)
![Guest OS](https://img.shields.io/badge/Guest%20OS-Ubuntu-E95420?style=for-the-badge)
![Benchmark](https://img.shields.io/badge/Benchmark-Sysbench-2ea44f?style=for-the-badge)
![Platform](https://img.shields.io/badge/Platform-Virtualization-blue?style=for-the-badge)
![Status](https://img.shields.io/badge/Status-Experimental-success?style=for-the-badge)

> A practical experimental study of **Type-1 and Type-2 hypervisors** using identically configured Ubuntu virtual machines and a common CPU benchmark workload.

---

## 📌 Project Overview

This project studies the performance of two different hypervisor architectures:

| Hypervisor             | Type       | Platform              |
| ---------------------- | ---------- | --------------------- |
| **Proxmox VE**         | **Type-1** | Bare-metal hypervisor |
| **VMware Workstation** | **Type-2** | Hosted hypervisor     |

The experiment creates an Ubuntu virtual machine on each platform using a controlled virtual hardware configuration and evaluates the VM using **Sysbench CPU benchmarking**.

The primary goal is to observe how the two virtualization approaches behave when the guest virtual machine is configured with comparable CPU, memory, and storage resources.

The supplied laboratory manual organizes the experiment into:

```text
PART A → Type-1 Hypervisor → Proxmox VE
PART B → Type-2 Hypervisor → VMware Workstation
```

The manual specifically states that the virtual machines should use the same basic configuration so that their performance can be studied comparatively.

---

# 🎯 Objectives

The objectives of this experiment are to:

* Understand the concepts of **Type-1 and Type-2 hypervisors**.
* Create an Ubuntu VM using **Proxmox VE**.
* Create an Ubuntu VM using **VMware Workstation**.
* Use controlled virtual hardware configurations.
* Verify CPU, memory, disk, network, and operating-system settings.
* Monitor VM resource utilization.
* Measure CPU performance using **Sysbench**.
* Record execution time, total events, throughput, and latency statistics.
* Preserve benchmark outputs and observations.
* Compare the measured behavior of the Type-1 and Type-2 environments.

---

# 🧠 Hypervisor Architecture

## Type-1 Hypervisor

A Type-1 hypervisor runs directly on the physical server hardware.

```text
┌───────────────────────────────┐
│       Physical Hardware       │
└───────────────┬───────────────┘
                │
                ▼
┌───────────────────────────────┐
│         Proxmox VE            │
│       Type-1 Hypervisor       │
└───────────────┬───────────────┘
                │
                ▼
┌───────────────────────────────┐
│          Ubuntu VM            │
│                               │
│  2 vCPU                       │
│  2 GB RAM                     │
│  20 GB Virtual Disk           │
│  vmbr0 Network                │
└───────────────────────────────┘
```

In this experiment, **Proxmox VE** represents the Type-1 environment. The manual describes Proxmox VE as being deployed on a centralized physical server and accessed through its web interface.

---

## Type-2 Hypervisor

A Type-2 hypervisor runs on top of a host operating system.

```text
┌───────────────────────────────┐
│       Physical Hardware       │
└───────────────┬───────────────┘
                │
                ▼
┌───────────────────────────────┐
│       Host Operating System   │
│            Windows            │
└───────────────┬───────────────┘
                │
                ▼
┌───────────────────────────────┐
│      VMware Workstation       │
│       Type-2 Hypervisor       │
└───────────────┬───────────────┘
                │
                ▼
┌───────────────────────────────┐
│          Ubuntu VM            │
│                               │
│  2 vCPU                       │
│  2 GB RAM                     │
│  20 GB Virtual Disk           │
│  NAT Network                  │
└───────────────────────────────┘
```

The manual identifies **VMware Workstation as the Type-2 hypervisor** and places it in Part B of the experiment.

---

# 🔍 Type-1 vs Type-2 at a Glance

| Feature                    | Type-1                        | Type-2                 |
| -------------------------- | ----------------------------- | ---------------------- |
| Hypervisor                 | **Proxmox VE**                | **VMware Workstation** |
| Architecture               | Bare-metal                    | Hosted                 |
| Host OS beneath hypervisor | No conventional host OS layer | Yes                    |
| Guest OS                   | Ubuntu                        | Ubuntu                 |
| CPU                        | 2 vCPU                        | 2 vCPU                 |
| Memory                     | 2 GB                          | 2 GB                   |
| Disk                       | 20 GB                         | 20 GB                  |
| Network                    | `vmbr0` bridge                | NAT                    |
| CPU Benchmark              | Sysbench                      | Sysbench               |
| Resource Monitoring        | Proxmox + Ubuntu tools        | VMware + Ubuntu tools  |

## The VM configurations above follow the configurations specified in the supplied manual.

# 🏗️ Experimental Design

The comparison follows this basic structure:

```text
                    PERFORMANCE ANALYSIS
                            │
               ┌────────────┴────────────┐
               │                         │
               ▼                         ▼
       TYPE-1 HYPERVISOR         TYPE-2 HYPERVISOR
          Proxmox VE             VMware Workstation
               │                         │
               ▼                         ▼
          Ubuntu VM                 Ubuntu VM
               │                         │
               └────────────┬────────────┘
                            │
                     SAME CORE WORKLOAD
                            │
                            ▼
                    Sysbench CPU Test
                            │
                            ▼
                  Performance Measurements
                            │
                            ▼
                 Type-1 / Type-2 Comparison
```

---

# 🖥️ Experimental Configuration

## Common VM Configuration

The manual specifies a common guest configuration for both environments:

```text
Operating System → Ubuntu
CPU              → 2 vCPU
Memory           → 2 GB
Disk             → 20 GB
```

## This allows the hypervisor environments to be studied using comparable guest resources.

## Type-1 Configuration — Proxmox VE

| Parameter       | Configuration                |
| --------------- | ---------------------------- |
| Hypervisor      | Proxmox VE                   |
| Hypervisor Type | Type-1                       |
| Guest OS        | Ubuntu                       |
| CPU             | 1 socket × 2 cores           |
| Total vCPU      | 2                            |
| Memory          | 2048 MiB / 2 GB              |
| Disk            | 20 GB                        |
| Storage         | local-lvm / assigned storage |
| Network Bridge  | `vmbr0`                      |
| Network Model   | Default / VirtIO             |
| Graphics        | Default                      |
| Machine         | Default                      |
| BIOS            | Default                      |
| SCSI Controller | Default                      |

The manual specifies the Proxmox VM workflow as:

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

---

## Type-2 Configuration — VMware Workstation

| Parameter        | Configuration                |
| ---------------- | ---------------------------- |
| Hypervisor       | VMware Workstation           |
| Hypervisor Type  | Type-2                       |
| Host OS          | Windows                      |
| Guest OS         | Ubuntu                       |
| CPU              | 1 processor × 2 cores        |
| Total vCPU       | 2                            |
| Memory           | 2048 MB / 2 GB               |
| Disk             | 20 GB                        |
| Network          | NAT                          |
| VM Configuration | Typical                      |
| Disk Storage     | Single file / default option |

The VMware Workstation configuration is specified in the manual's Type-2 section.

---

# 🌐 Type-1 — Proxmox VE Setup

## 1. Access Proxmox VE

Open a web browser and navigate to:

```text
https://<PROXMOX_SERVER_IP>:8006
```

Example:

```text
https://192.168.X.X:8006
```

The Proxmox server IP address and login credentials must be available before starting the experiment.

---

## 2. Log In

If the browser shows a certificate warning:

```text
Advanced
   ↓
Proceed to <Server IP>
   ↓
Proxmox Login
```

Enter the assigned credentials and access the Proxmox management dashboard.

---

## 3. Understand the Proxmox Interface

The manual describes the hierarchy as:

```text
Datacenter
    │
    └── Proxmox Node
          ├── Virtual Machines
          ├── Storage
          └── Network
```

The interface also provides access to CPU and memory utilization information.

---

# 🛠️ Creating the Type-1 Virtual Machine

## VM Creation Workflow

```text
Select Proxmox Node
        ↓
Create VM
        ↓
Configure General Settings
        ↓
Select Ubuntu ISO
        ↓
Configure System
        ↓
Configure 20 GB Disk
        ↓
Configure 2 vCPU
        ↓
Configure 2 GB RAM
        ↓
Configure vmbr0
        ↓
Confirm
        ↓
Finish
```

### Recommended naming convention

```text
<Name>-Type1
```

Example from the manual:

```text
CC-Experiment1-Type1
```

The manual specifies the naming convention and VM creation stages in the Proxmox section.

---

# ✅ Type-1 VM Verification

After the Ubuntu VM is created and started, verify the guest configuration.

## Operating System

```bash
hostnamectl
```

Check:

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

Check:

```text
Architecture
CPU(s)
CPU Model
Virtualization Information
```

Expected allocation:

```text
2 vCPU
```

## Memory

```bash
free -h
```

Check:

```text
Total Memory
Used Memory
Free Memory
Available Memory
```

Expected allocation:

```text
2 GB
```

## Disk

```bash
df -h
```

Check:

```text
Filesystem
Total Capacity
Used Space
Available Space
```

These checks are directly specified in the Type-1 workflow.

---

# 📊 Type-1 Resource Monitoring

Inside Ubuntu:

```bash
top
```

Observe:

```text
CPU utilization
Memory utilization
Running processes
Load average
```

From the Proxmox interface, navigate to:

```text
Datacenter
   ↓
Proxmox Node
   ↓
Virtual Machine
   ↓
Summary
```

Observe:

```text
CPU Usage
Memory Usage
Network Traffic
Disk Usage
```

The manual specifically asks these Proxmox-level resource observations to be recorded for later comparison with the Type-2 environment.

---

# ⚙️ Type-1 CPU Benchmark

Install Sysbench:

```bash
sudo apt update
sudo apt install sysbench -y
```

Verify:

```bash
sysbench --version
```

Run:

```bash
sysbench cpu --cpu-max-prime=20000 run
```

Record:

```text
Total Execution Time
Total Number of Events
Events per Second
Latency Statistics
Average Latency
```

The Sysbench CPU workload and required measurements are specified in the Type-1 section.

---

# 💻 Type-2 — VMware Workstation Setup

## 1. Launch VMware Workstation

Open VMware Workstation and select:

```text
Create a New Virtual Machine
```

---

## 2. Select Typical Configuration

Choose:

```text
Typical (recommended)
```

---

## 3. Select Ubuntu ISO

Choose:

```text
Installer disc image file (iso)
```

and select the required Ubuntu ISO.

---

## 4. Configure Guest OS

Use:

```text
Guest OS       → Linux
Version        → Ubuntu 64-bit
```

---

## 5. Name the VM

Recommended manual naming convention:

```text
CC-Experiment1-Type2
```

---

## 6. Configure Virtual Disk

Set:

```text
Maximum Disk Size → 20 GB
```

Use the default/single-file storage option described in the manual.

---

## 7. Customize Hardware

Configure:

```text
Memory:
2048 MB

Processors:
1 processor × 2 cores

Total:
2 vCPU

Network:
NAT
```

The VMware hardware configuration is explicitly given in the supplied manual.

---

# ✅ Type-2 VM Verification

After installing Ubuntu:

```bash
hostnamectl
lscpu
free -h
df -h
```

Verify:

```text
Hostname
Operating System
Kernel
Architecture
CPU configuration
Memory configuration
Disk configuration
```

Expected:

```text
2 vCPU
2 GB RAM
20 GB disk
```

The manual also asks the user to verify the virtualization-related CPU information using `lscpu`.

---

# 📊 Type-2 Resource Monitoring

Inside Ubuntu:

```bash
top
```

Observe:

```text
CPU utilization
Memory utilization
Running processes
Load average
```

VMware Workstation can also be used to verify:

```text
Processors
Memory
Hard Disk
Network Adapter
```

The manual describes both VMware hardware verification and guest-level monitoring.

---

# ⚙️ Type-2 CPU Benchmark

Install Sysbench:

```bash
sudo apt update
sudo apt install sysbench -y
```

Verify:

```bash
sysbench --version
```

Run:

```bash
sysbench cpu --cpu-max-prime=20000 run
```

Record:

```text
Total Execution Time
Total Number of Events
Events per Second
Minimum Latency
Average Latency
Maximum Latency
```

These are the measurements required by the Type-2 benchmark section of the manual.

---

# 📈 Results

> **Replace the placeholder paths below with the actual files from your `results/` directory. Do not change the measured values to examples from the manual.**

## Type-1 — Proxmox VE

### Raw Result

📄 [View Type-1 Raw CPU Result](results/type1/raw/cpu.txt)

### Processed Result

📊 [View Type-1 Processed CPU Result](results/type1/processed/cpu_results.csv)

### Performance Graph

📈 [View Type-1 CPU Performance Graph](results/type1/figures/cpu_performance.png)

### Observation

| Parameter            | Type-1 Result           |
| -------------------- | ----------------------- |
| Hypervisor           | Proxmox VE              |
| Hypervisor Type      | Type-1                  |
| Guest OS             | Ubuntu                  |
| CPU                  | 2 vCPU                  |
| Memory               | 2 GB                    |
| Disk                 | 20 GB                   |
| Total Execution Time | **[ADD ACTUAL RESULT]** |
| Total Events         | **[ADD ACTUAL RESULT]** |
| Events/sec           | **[ADD ACTUAL RESULT]** |
| Average Latency      | **[ADD ACTUAL RESULT]** |

---

## Type-2 — VMware Workstation

### Raw Result

📄 [View Type-2 Raw CPU Result](results/type2/raw/cpu.txt)

### Processed Result

📊 [View Type-2 Processed CPU Result](results/type2/processed/cpu_results.csv)

### Performance Graph

📈 [View Type-2 CPU Performance Graph](results/type2/figures/cpu_performance.png)

### Observation

| Parameter            | Type-2 Result           |
| -------------------- | ----------------------- |
| Hypervisor           | VMware Workstation      |
| Hypervisor Type      | Type-2                  |
| Guest OS             | Ubuntu                  |
| CPU                  | 2 vCPU                  |
| Memory               | 2 GB                    |
| Disk                 | 20 GB                   |
| Network              | NAT                     |
| Total Execution Time | 10.0004 s |
| Total Events         | 333929 |
| Events/sec           | 33381.54 |
| Minimum Latency      | 0.01 ms |
| Average Latency      | 0.03 ms |
| Maximum Latency      | 5.54 ms |

---

# 🆚 Final Type-1 vs Type-2 Comparison

Populate this table only after inserting the actual measured benchmark values.

| Metric               | Proxmox VE — Type-1 | VMware Workstation — Type-2 |      Difference |
| -------------------- | ------------------: | --------------------------: | --------------: |
| Total Execution Time |           **[ADD]** |                   **[ADD]** | **[CALCULATE]** |
| Total Events         |           **[ADD]** |                   **[ADD]** | **[CALCULATE]** |
| Events per Second    |           **[ADD]** |                   **[ADD]** | **[CALCULATE]** |
| Minimum Latency      |           **[ADD]** |                   **[ADD]** | **[CALCULATE]** |
| Average Latency      |           **[ADD]** |                   **[ADD]** | **[CALCULATE]** |
| Maximum Latency      |           **[ADD]** |                   **[ADD]** | **[CALCULATE]** |

### Comparison Graph

📊 [View Type-1 vs Type-2 Comparison Graph](results/comparison/type1_vs_type2_cpu.png)

> Replace the placeholder path with your actual comparison figure.

---

# 🧮 Performance Difference

For throughput:

```text
Difference (%) =
((Type-2 Throughput - Type-1 Throughput)
 / Type-1 Throughput) × 100
```

For execution time:

```text
Difference (%) =
((Type-1 Time - Type-2 Time)
 / Type-1 Time) × 100
```

Use the formula appropriate to the metric and report the actual measured values.

---

# 🔬 Observation Framework

The experiment evaluates the two virtualization approaches using the following observation categories:

```text
Virtualization Architecture
        ↓
VM Configuration
        ↓
CPU Configuration
        ↓
Memory Configuration
        ↓
Disk Configuration
        ↓
Network Configuration
        ↓
Resource Utilization
        ↓
CPU Benchmark
        ↓
Performance Metrics
        ↓
Type-1 / Type-2 Comparison
```

---

# 🔁 Complete Experimental Workflow

```text
                         START
                           │
                           ▼
                Understand Hypervisors
                           │
             ┌─────────────┴─────────────┐
             │                           │
             ▼                           ▼
        TYPE-1                         TYPE-2
      Proxmox VE                 VMware Workstation
             │                           │
             ▼                           ▼
       Access Web UI               Launch VMware
             │                           │
             ▼                           ▼
        Create VM                  Create New VM
             │                           │
             ▼                           ▼
        Ubuntu ISO                   Ubuntu ISO
             │                           │
             ▼                           ▼
        2 vCPU                       2 vCPU
             │                           │
             ▼                           ▼
         2 GB RAM                     2 GB RAM
             │                           │
             ▼                           ▼
         20 GB Disk                   20 GB Disk
             │                           │
             ▼                           ▼
          vmbr0                          NAT
             │                           │
             └─────────────┬─────────────┘
                           │
                           ▼
                     Install Ubuntu
                           │
                           ▼
                    Verify Configuration
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
                  Record Measurements
                           │
                           ▼
                  Store Actual Results
                           │
                           ▼
                 Compare Type-1 / Type-2
                           │
                           ▼
                          END
```

---

# 📂 Suggested Repository Organization

```text
type1-type2-hypervisor-performance/
│
├── README.md
│
├── docs/
│   ├── type1/
│   │   ├── vm-configuration.md
│   │   └── screenshots/
│   │
│   └── type2/
│       ├── vm-configuration.md
│       └── screenshots/
│
├── results/
│   ├── type1/
│   │   ├── raw/
│   │   ├── processed/
│   │   └── figures/
│   │
│   ├── type2/
│   │   ├── raw/
│   │   ├── processed/
│   │   └── figures/
│   │
│   └── comparison/
│       ├── processed/
│       └── figures/
│
└── scripts/
    ├── run_type1_benchmark.sh
    ├── run_type2_benchmark.sh
    └── analyze_results.py
```

> Modify this structure to match the folders that actually exist in the final repository.

---

# 🧪 Reproducibility

## Type-1 — Proxmox VE

### Access

```text
https://<PROXMOX_SERVER_IP>:8006
```

### VM Configuration

```text
Guest OS  → Ubuntu
CPU       → 2 vCPU
Memory    → 2 GB
Disk      → 20 GB
Network   → vmbr0
```

### Verify

```bash
hostnamectl
lscpu
free -h
df -h
```

### Monitor

```bash
top
```

### Benchmark

```bash
sudo apt update
sudo apt install sysbench -y
sysbench --version
sysbench cpu --cpu-max-prime=20000 run
```

---

## Type-2 — VMware Workstation

### VM Configuration

```text
Guest OS  → Ubuntu
CPU       → 2 vCPU
Memory    → 2 GB
Disk      → 20 GB
Network   → NAT
```

### Verify

```bash
hostnamectl
lscpu
free -h
df -h
```

### Monitor

```bash
top
```

### Benchmark

```bash
sudo apt update
sudo apt install sysbench -y
sysbench --version
sysbench cpu --cpu-max-prime=20000 run
```

---

# 📋 Experiment Checklist

## Type-1 — Proxmox VE

```text
☐ Access Proxmox VE
☐ Log in to Proxmox
☐ Select Proxmox Node
☐ Create VM
☐ Select Ubuntu ISO
☐ Configure 2 vCPU
☐ Configure 2 GB RAM
☐ Configure 20 GB disk
☐ Configure vmbr0
☐ Start VM
☐ Install Ubuntu
☐ Verify hostnamectl
☐ Verify lscpu
☐ Verify memory using free -h
☐ Verify disk using df -h
☐ Monitor using top
☐ Install Sysbench
☐ Run CPU benchmark
☐ Record benchmark results
☐ Record Proxmox resource observations
☐ Save result files
☐ Shut down VM
```

## Type-2 — VMware Workstation

```text
☐ Launch VMware Workstation
☐ Create New Virtual Machine
☐ Select Typical configuration
☐ Select Ubuntu ISO
☐ Configure 2 vCPU
☐ Configure 2 GB RAM
☐ Configure 20 GB disk
☐ Configure NAT
☐ Finish VM creation
☐ Power on VM
☐ Install Ubuntu
☐ Verify hostnamectl
☐ Verify lscpu
☐ Verify memory using free -h
☐ Verify disk using df -h
☐ Monitor using top
☐ Install Sysbench
☐ Run CPU benchmark
☐ Record benchmark results
☐ Save result files
☐ Shut down VM
```

---

# 📊 Result Integrity

The benchmark values reported in this repository should always come from the **actual experiment output**.

The intended evidence chain is:

```text
Actual VM Configuration
          ↓
Actual Benchmark Execution
          ↓
Raw Benchmark Output
          ↓
Processed Result
          ↓
Calculated Comparison
          ↓
Observed Conclusion
```

Example values shown in the laboratory manual are configuration or format examples and should not be presented as experimental measurements.

---

# 💡 Key Learning Outcomes

By completing this experiment, the learner gains practical understanding of:

### Virtualization

* Type-1 hypervisor architecture
* Type-2 hypervisor architecture
* Virtual machine resource allocation
* Guest operating-system isolation

### System Administration

* VM creation
* CPU configuration
* Memory allocation
* Virtual disk configuration
* Network configuration
* System verification

### Performance Analysis

* CPU benchmarking with Sysbench
* Execution-time measurement
* Throughput measurement
* Latency measurement
* Resource monitoring
* Comparative analysis

### Experimental Practice

* Controlled configurations
* Repeatable benchmark execution
* Recording actual measurements
* Preserving raw evidence
* Comparing measured performance

---

# 📌 Important Comparison Note

The two experiments use comparable guest CPU, memory, and disk allocations, but the network configurations specified by the manual differ:

```text
Proxmox VE      → vmbr0
VMware          → NAT
```

Therefore, network-specific observations should be interpreted in the context of these configured modes rather than assuming identical network paths.

---

# 🏁 Conclusion

This laboratory experiment provides a practical comparison framework for studying **Type-1 and Type-2 hypervisors**.

The Type-1 environment uses **Proxmox VE**, while the Type-2 environment uses **VMware Workstation**. Both environments host Ubuntu virtual machines configured with **2 vCPU, 2 GB RAM, and 20 GB disk space** according to the laboratory procedure.

The CPU benchmark is performed using the same Sysbench workload:

```text
sysbench cpu --cpu-max-prime=20000 run
```

The final comparison is based on the actual recorded execution time, total events, events per second, and latency statistics.

> **Final conclusions should be drawn only after inserting the actual measurements collected from both hypervisor environments.**

---

# 📚 Reference

**Performance Analysis of Type-1 and Type-2 Hypervisors — Lab Manual**

### Part A

**Performance Analysis Using Type-1 Hypervisor – Proxmox VE**

### Part B

**Performance Analysis Using Type-2 Hypervisor – VMware Workstation**

## This README follows the supplied laboratory manual's Type-1 and Type-2 workflows, VM configurations, verification procedure, monitoring approach, Sysbench CPU benchmark, observation tables, and comparison framework.

# 👩‍💻 Project Information

**Project:** Performance Analysis of Type-1 and Type-2 Hypervisors

**Type-1:** Proxmox VE
**Type-2:** VMware Workstation
**Guest OS:** Ubuntu
**Benchmark:** Sysbench CPU
**Primary Metrics:** Execution Time, Total Events, Events/sec, Latency

---

### ⭐ Experimental Principle

> **Measure first. Compare second. Conclude from evidence.**
