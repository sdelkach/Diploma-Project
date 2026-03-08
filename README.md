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
    subgraph INTERNET["🌐 INTERNET<br/><b>Public Network</b>"]
        direction TB
        INT[("fa:fa-cloud Internet")]
    end

    subgraph DMZ["🛡️ DMZ ZONE<br/><b>VLAN 503 | 10.0.503.0/24</b>"]
        direction TB
        MT["fa:fa-shield-halved MikroTik CCR1036<br/><b>Border Router + Firewall</b>"]
        NGX["fa:fa-server ZB-NGINX<br/><b>Reverse Proxy</b><br/>443/TCP, 80/TCP"]
    end

    subgraph INTERNAL["🔒 INTERNAL ZONE<br/><b>VLAN 669 | 10.0.669.0/24</b>"]
        direction TB
        
        subgraph MANAGEMENT["⚙️ Management Layer"]
            BLESTA["fa:fa-file-invoice-dollar ZB-BLESTA<br/><b>Billing Panel</b><br/>80/TCP, 443/TCP"]
            VZ["fa:fa-sliders VZ-MASTER<br/><b>Virtualizor Control</b><br/>4083/TCP, 4085/TCP"]
        end
        
        subgraph COMPUTE["🖥️ Compute Layer"]
            direction LR
            PVE["fa:fa-server PROXMOX CLUSTER<br/><b>HA Enabled | Ceph Storage</b>"]
            
            N1["fa:fa-server Node 1<br/><b>101-3-28</b><br/>HPE DL360p Gen8"]
            N2["fa:fa-server Node 2<br/><b>101-3-29</b><br/>HPE DL360p Gen8"]
            N3["fa:fa-server Node 3<br/><b>101-3-30</b><br/>HPE DL360p Gen8"]
        end
        
        subgraph STORAGE["💾 Storage Layer"]
            LVM[("fa:fa-disk LVM Storage<br/><b>Distributed | Thin Provisioning</b>")]
        end
    end

    %% Connections
    INT ==>|"<b>1 Gbps</b><br/>Public Traffic"| MT
    MT ==>|"<b>Firewall Rules</b><br/>NAT, ACL"| NGX
    NGX ==>|"<b>10 Gbps BOND</b><br/>Aggregated Link"| BLESTA
    NGX ==>|"<b>10 Gbps BOND</b><br/>Aggregated Link"| VZ
    BLESTA -.->|"<b>API</b><br/>JSON-RPC"| VZ
    VZ ==>|"<b>API</b><br/>Proxmox VE 8.4"| PVE
    PVE --> N1
    PVE --> N2
    PVE --> N3
    N1 & N2 & N3 ==> LVM

    %% Styling
    classDef internet fill:#3498DB,stroke:#2980B9,stroke-width:2px,color:#fff,font-weight:bold
    classDef dmz fill:#E74C3C,stroke:#C0392B,stroke-width:2px,color:#fff,font-weight:bold
    classDef internal fill:#2ECC71,stroke:#27AE60,stroke-width:2px,color:#fff,font-weight:bold
    classDef management fill:#9B59B6,stroke:#8E44AD,stroke-width:2px,color:#fff
    classDef compute fill:#1ABC9C,stroke:#16A085,stroke-width:2px,color:#fff
    classDef storage fill:#95A5A6,stroke:#7F8C8D,stroke-width:2px,color:#fff

    class INT internet
    class MT,NGX dmz
    class BLESTA,VZ management
    class PVE,N1,N2,N3 compute
    class LVM storage
