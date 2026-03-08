# Diploma-Project
Billing integration with Proxmox Virtualization.

# Proxmox VE Billing Integration System

> Automated VPS provisioning and billing system for Data Center infrastructure

![Proxmox](https://img.shields.io/badge/Proxmox-VE8.4-orange)
![Virtualizor](https://img.shields.io/badge/Virtualizor-KVM-green)
![Blesta](https://img.shields.io/badge/Blesta-Billing-blue)
![Linux](https://img.shields.io/badge/OS-Ubuntu%2FDebian-black)
---

## 📋 Table of Contents

- [Overview](#-overview)
- [Architecture](#-architecture)
- [Hardware Specifications](#-hardware-specifications)
- [Virtual Infrastructure](#-virtual-infrastructure)
- [Implementation](#-implementation)
- [Integration](#-integration)
- [Economic Analysis](#-economic-analysis)
- [Results](#-results)
- [Documentation](#-documentation)
- [Author](#-author)

---

## 🎯 Overview

### Project Goal
Automate VPS lifecycle management with integrated billing for Data Center operations.

### Business Problem
| Problem | Impact | Solution |
|---------|--------|----------|
| Manual VM provisioning | 1 day deployment time | Automated templates (1-2 hours) |
| No resource tracking | Billing inaccuracies | Proxmox + Virtualizor metrics |
| VMware license costs | High operational expenses | Proxmox VE (open-source) |

### Solution Architecture
Proxmox VE + Virtualizor + Blesta integration for full automation.

### Scope
- **3 physical nodes** (HPE DL360p Gen8)
- **35+ client migration plan**
- **99.9% SLA target**
- **10 months payback period**

---

## 🏗️ Architecture

### Network Topology

<div align="center"><img width="974" height="299" alt="image" src="https://github.com/user-attachments/assets/fc1ad91c-0df1-462f-85d7-59dd5324e84d" />

### VLAN Segmentation

| VLAN ID | Purpose | IP Range | Components |
|---------|---------|----------|------------|
| VLAN 503(ext) | Billing | 95.2xx.1xx.0/24 | Nginx proxy, Public IP |
| VLAN 669(int) | Management | 172.25.3.0/16 | Proxmox, Virtualizor, Blesta |

### Component Flow

![Architecture](docs/architecture.svg)

### HPE ProLiant DL360p Gen8 - Front View
<div align="center"><img width="652" height="117" alt="image" src="https://github.com/user-attachments/assets/b4c0d58e-5db2-4108-9a8c-9af6e58f64d3" />

### HPE ProLiant DL360p Gen8 - Rear View (Power & Network)
<div align="center"><img width="652" height="162" alt="image" src="https://github.com/user-attachments/assets/806854d4-6324-4ecd-b322-d690adb5f147" />

## Technical Specifications

| Parameter | Specification |
|-----------|---------------|
| **Model** | HPE ProLiant DL360p Gen8 |
| **Form Factor** | 1U Rack Server |
| **CPU** | 2 × Intel Xeon E5-2697 v2 (12C / 24T each) |
| **Total CPU Threads** | 48 |
| **RAM** | 256 GB DDR3 ECC Registered |
| **Maximum RAM** | 768 GB |
| **Storage Bays** | 8 × 2.5" SFF SAS/SATA |
| **RAID Controller** | HP Smart Array P420i |
| **Network Interfaces** | 4 × 1GbE + optional 10GbE |
| **Remote Management** | HP iLO 4 |
| **Power Supply** | 2 × 460W Hot-Plug (Redundant) |
| **Virtualization Support** | VT-x, VT-d, SR-IOV |

## Rack Configuration

| Rack Unit | Component | Power Supply |
|----------|-----------|--------------|
| 1U | HPE DL360p Gen8 (Node 1) | 2 × 460W |
| 1U | HPE DL360p Gen8 (Node 2) | 2 × 460W |
| 1U | HPE DL360p Gen8 (Node 3) | 2 × 460W |
| Network | MikroTik CCR1036 | 1 × PSU |
| Virtual Layer | Nginx / Blesta / Virtualizor VMs | Shared |

## Virtual Infrastructure

### VM Resource Allocation

| VM | vCPU | RAM | Storage | Network | Purpose |
|----|------|-----|---------|---------|---------|
| **Nginx** | 2 | 4 GB | 50 GB | VLAN 503 + 669 | Reverse Proxy |
| **Blesta** | 2 | 4 GB | 65 GB | VLAN 669 | Billing System |
| **Virtualizor** | 3 | 6 GB | 100 GB | VLAN 503 + 669 | VPS Management |

#### Nginx VM
<div align="center"><img width="1191" height="467" alt="image" src="https://github.com/user-attachments/assets/f824082b-e4e3-4428-9e40-58686fcb9561" />


#### Virtualizor VM
<div align="center"><img width="1126" height="465" alt="image" src="https://github.com/user-attachments/assets/066de80b-4bd9-4c8e-b007-b8ef211d1ac2" />


#### Blesta VM
<div align="center"><img width="1050" height="423" alt="image" src="https://github.com/user-attachments/assets/a1284dbf-e247-4fdd-b770-687c36e1b94d" />



