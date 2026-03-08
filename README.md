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
| External | Client VPS | 95.2xx.1xx.0/24 | Public IP pool |

### Component Flow

![Architecture](docs/architecture.svg)
<svg width="900" height="720" viewBox="0 0 900 720" xmlns="http://www.w3.org/2000/svg" font-family="Arial">

<style>
.box { fill:#2c3e50; stroke:#1a252f; stroke-width:2; rx:10 }
.dmz { fill:#e74c3c }
.internal { fill:#2ecc71 }
.mgmt { fill:#8e44ad }
.compute { fill:#16a085 }
.storage { fill:#7f8c8d }
.text { fill:white; font-size:14px; text-anchor:middle }
.title { fill:#ecf0f1; font-size:18px; font-weight:bold; text-anchor:middle }
.line { stroke:#bdc3c7; stroke-width:2 }
</style>

<!-- Internet -->
<text x="450" y="30" class="title">Internet</text>
<rect x="375" y="50" width="150" height="50" class="box"/>
<text x="450" y="80" class="text">Internet</text>

<!-- DMZ -->
<text x="450" y="140" class="title">DMZ | VLAN 503</text>

<rect x="330" y="160" width="240" height="60" class="box dmz"/>
<text x="450" y="190" class="text">MikroTik CCR1036</text>
<text x="450" y="210" class="text">Border Router / Firewall</text>

<rect x="330" y="240" width="240" height="60" class="box dmz"/>
<text x="450" y="270" class="text">ZB-NGINX</text>
<text x="450" y="290" class="text">Reverse Proxy 80 / 443</text>

<!-- Management -->
<text x="450" y="350" class="title">Management Layer</text>

<rect x="230" y="370" width="200" height="70" class="box mgmt"/>
<text x="330" y="400" class="text">ZB-BLESTA</text>
<text x="330" y="420" class="text">Billing Panel</text>

<rect x="470" y="370" width="200" height="70" class="box mgmt"/>
<text x="570" y="400" class="text">VZ-MASTER</text>
<text x="570" y="420" class="text">Virtualizor</text>

<!-- Proxmox -->
<text x="450" y="480" class="title">Compute Layer</text>

<rect x="330" y="500" width="240" height="70" class="box compute"/>
<text x="450" y="530" class="text">Proxmox Cluster</text>
<text x="450" y="550" class="text">HA | Ceph</text>

<!-- Nodes -->
<rect x="180" y="600" width="160" height="60" class="box compute"/>
<text x="260" y="630" class="text">Node 1</text>
<text x="260" y="648" class="text">HPE DL360p</text>

<rect x="370" y="600" width="160" height="60" class="box compute"/>
<text x="450" y="630" class="text">Node 2</text>
<text x="450" y="648" class="text">HPE DL360p</text>

<rect x="560" y="600" width="160" height="60" class="box compute"/>
<text x="640" y="630" class="text">Node 3</text>
<text x="640" y="648" class="text">HPE DL360p</text>

<!-- Storage -->
<text x="450" y="700" class="title">Distributed LVM Storage</text>

<!-- Lines -->
<line x1="450" y1="100" x2="450" y2="160" class="line"/>
<line x1="450" y1="220" x2="450" y2="240" class="line"/>
<line x1="450" y1="300" x2="330" y2="370" class="line"/>
<line x1="450" y1="300" x2="570" y2="370" class="line"/>

<line x1="570" y1="440" x2="450" y2="500" class="line"/>

<line x1="450" y1="570" x2="260" y2="600" class="line"/>
<line x1="450" y1="570" x2="450" y2="600" class="line"/>
<line x1="450" y1="570" x2="640" y2="600" class="line"/>

</svg>
