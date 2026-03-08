# Diploma-Project
Billing integration with Proxmox Virtualization.

# Proxmox VE Billing Integration System

> Automated VPS provisioning and billing system for Data Center infrastructure

![Proxmox](https://img.shields.io/badge/Proxmox-VE8.4-orange)
![Virtualizor](https://img.shields.io/badge/Virtualizor-KVM-green)
![Blesta](https://img.shields.io/badge/Blesta-Billing-blue)
![Linux](https://img.shields.io/badge/OS-Ubuntu%2FDebian-black)

---

## 📋 Overview

This project implements an automated billing system integrated with Proxmox VE virtualization platform. The solution enables Data Centers to provide VPS services with automatic provisioning, resource tracking, and invoice generation.

**Live Demo:** [Link to demo if available]  
**Documentation:** [See /docs folder]

---

## 🎯 Key Features

- ✅ Automated VPS provisioning via Blesta + Virtualizor API
- ✅ Resource-based billing (CPU, RAM, Storage, Bandwidth)
- ✅ Multi-node Proxmox cluster support (3x HPE DL360p Gen8)
- ✅ Client self-service portal
- ✅ Automated notifications (Email, Telegram)
- ✅ 99.9% uptime SLA compliance

---

## 🏗️ Architecture

<img width="974" height="299" alt="image" src="https://github.com/user-attachments/assets/fc1ad91c-0df1-462f-85d7-59dd5324e84d" />


---

## 🛠️ Tech Stack

| Component | Technology |
|-----------|------------|
| Virtualization | Proxmox VE 8.4, KVM, LXC |
| Management | Virtualizor Master Panel |
| Billing | Blesta 5.x |
| Web Server | Nginx (Reverse Proxy) |
| Hardware | HPE ProLiant DL360p Gen8 (3 nodes) |
| OS | Ubuntu 20.04 LTS, Debian 12 |
| Monitoring | Zabbix 6.0 + Wirenboard |

---

## 📊 Results

| Metric | Value |
|--------|-------|
| Deployment Time | 1-2 hours (via templates) |
| VM Rollout | Fully automated |
| Billing Accuracy | 100% (resource-based) |
| Cluster Nodes | 3 physical servers |
| Max Concurrent VMs | 20+ (tested) |
| Payback Period | ~10 months |

---

## 📁 Project Structure

├── README.md # This file
├── docs/ # Full documentation
├── configs/ # Configuration examples
├── diagrams/ # Network diagrams
└── scripts/ # Automation scripts
