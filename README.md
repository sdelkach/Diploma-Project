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

<img width="974" height="299" alt="image" src="https://github.com/user-attachments/assets/fc1ad91c-0df1-462f-85d7-59dd5324e84d" />

### VLAN Segmentation

| VLAN ID | Purpose | IP Range | Components |
|---------|---------|----------|------------|
| VLAN 503 | Management | 10.0.503.0/24 | Proxmox, Virtualizor, Nginx |
| VLAN 669 | Billing | 10.0.669.0/24 | Blesta, Internal Services |
| External | Client VPS | 95.214.117.0/24 | Public IP pool |

### Component Flow

```mermaid
flowchart TB
    Client[Client Browser] --> Nginx[Nginx Reverse Proxy]
    Nginx --> Blesta[Blesta Billing Panel]
    Nginx --> Virtualizor[Virtualizor Master]
    Blesta -->|API| Virtualizor
    Virtualizor -->|API| Proxmox[Proxmox Cluster]
    Proxmox --> Node1[Node 1: 101-3-28]
    Proxmox --> Node2[Node 2: 101-3-29]
    Proxmox --> Node3[Node 3: 101-3-30]
    
    style Client fill:#3498DB,stroke:#2980B9,color:#fff
    style Nginx fill:#F39C12,stroke:#D35400,color:#fff
    style Blesta fill:#9B59B6,stroke:#8E44AD,color:#fff
    style Virtualizor fill:#1ABC9C,stroke:#16A085,color:#fff
    style Proxmox fill:#2ECC71,stroke:#27AE60,color:#fff

