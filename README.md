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

*Network architecture diagram*
<img width="974" height="299" alt="image" src="https://github.com/user-attachments/assets/fc1ad91c-0df1-462f-85d7-59dd5324e84d" />

### VLAN Segmentation

| VLAN ID | Purpose | IP Range | Components |
|---------|---------|----------|------------|
| VLAN 503(ext) | Billing | 95.2xx.1xx.0/24 | Nginx proxy, Public IP |
| VLAN 669(int) | Management | 172.25.3.0/16 | Proxmox, Virtualizor, Blesta |

### Component Flow

![Architecture](docs/architecture.svg)

### HPE ProLiant DL360p Gen8 - Front View
<img width="652" height="117" alt="image" src="https://github.com/user-attachments/assets/b4c0d58e-5db2-4108-9a8c-9af6e58f64d3" />

### HPE ProLiant DL360p Gen8 - Rear View (Power & Network)
<img width="652" height="162" alt="image" src="https://github.com/user-attachments/assets/806854d4-6324-4ecd-b322-d690adb5f147" />

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
<img width="1191" height="467" alt="image" src="https://github.com/user-attachments/assets/f824082b-e4e3-4428-9e40-58686fcb9561" />


#### Virtualizor VM
<img width="1126" height="465" alt="image" src="https://github.com/user-attachments/assets/066de80b-4bd9-4c8e-b007-b8ef211d1ac2" />


#### Blesta VM
<img width="1050" height="423" alt="image" src="https://github.com/user-attachments/assets/a1284dbf-e247-4fdd-b770-687c36e1b94d" />


## Configuration  & setup

### Nginx Reverse Proxy
#### 1) Installation nginx & utilities
```bash
sudo apt update

sudo apt install -y nginx nginx-extras libmodsecurity3
```
#### 2) Configuration File:
> /etc/nginx/sites-available/billing

<img width="1257" height="543" alt="image" src="https://github.com/user-attachments/assets/3c3d79f8-dae9-4643-b94d-22fa83e6b1e1" />

### Blesta Billing System

#### 1) Installation

```bash
sudo apt update
sudo apt upgrade -y

sudo apt install -y \
php \
php-mysql \
php-curl \
php-xml \
php-mbstring \
php-gd
```
Full blesta installation guide is here: https://docs.blesta.com/installation

#### 2) Setup 
(This step should begining after virtualizor installation is already completed!!!)

Open in browser:
```
https://your-domain.com/blesta (short path provided by our nginx reverse proxy)
```
And go to the packages settings, so, for Blesta, Virtualizor have official packages which installed by instructions:
https://www.virtualizor.com/docs/billing/blesta-module/
And, here it is:
<img width="957" height="376" alt="image" src="https://github.com/user-attachments/assets/a500733a-0aa1-445e-95a2-e05e81881007" />


### Virtualizor Panel
#### 1) Installation

(Just download official script and choose right options, after that press enter and wait)
<img width="862" height="127" alt="image" src="https://github.com/user-attachments/assets/abb30475-971e-4310-b6bd-3cbc632c4b15" />
<img width="606" height="175" alt="image" src="https://github.com/user-attachments/assets/f0561ec2-41be-4014-8acc-cdc607d27d0c" />
Now, restart VM for complete changes and open in browser(https://external_ip/).
<img width="1161" height="582" alt="image" src="https://github.com/user-attachments/assets/d85e6dcd-afe4-46ce-a633-61903b3edccf" />
LogIn and checking important info in panel:
<img width="1131" height="680" alt="image" src="https://github.com/user-attachments/assets/18894724-a326-4186-a8e2-c25897131755" />

#### 2) IP Pools
Virtualizor panel need to know which exactly networks should be used in working with commercial services provided by platform VM creation. Now, we needed add a two main networks in Virtualizor, which get access customers to their remote VMs(services).
Add through netplan complete network config on VIrtualizor VM.(remember that the one VLAN just for remote access, and other for pairs with performance cluster servers, which are store all VMs).
<img width="1156" height="480" alt="image" src="https://github.com/user-attachments/assets/6e0a77ea-8e5f-4a92-a16c-5393fe430f7b" />
Add ip pool, by recommended way in official docs: https://www.virtualizor.com/docs/admin-api/create-ip-pool/
External IP pool:
<img width="1287" height="686" alt="image" src="https://github.com/user-attachments/assets/c444f6e3-8a52-4972-b261-1ffc91499a0e" />
Internal IP pool:
<img width="1270" height="698" alt="image" src="https://github.com/user-attachments/assets/f3f3fc97-a528-420a-a121-6393400e4af5" />

After that, we early added media iso(just upload it from pc) for ending test of this whole project.
<img width="1142" height="286" alt="image" src="https://github.com/user-attachments/assets/328f8594-05a5-4a28-ada9-65c7a4cab252" />

Also i've added template(flexible paid tariff of service hosting).
<img width="1216" height="547" alt="image" src="https://github.com/user-attachments/assets/d76b6713-f4c0-47bf-a0cc-5cc6013bdcc3" />



### Proxmox Cluster nodes

>For complete all components of this project, we should have finish setuping pre-prepared server nodes, which have already proxmox installation and just needed Virtualizor install above them(important: kernel should be exactly Proxmox, not kvm or another, else work will broke). Cause earlier i show you how install through the script that panel, i've skip that step.

>After we've installed Virtualizor on every proxmox node in cluster(3 of 3), we must add from main server Virtualizor(VM) add slave servers(proxmox cluster nodes), and then our main server become a "Master server", from which we can setup any options on every slave nodes(needed some setup in API section(API token) inside proxmox):
<img width="861" height="405" alt="image" src="https://github.com/user-attachments/assets/4cdf77ff-8d1b-45ef-9922-1b3c4fe649e5" />

>After that, we have 2 values(API token name & secret key), go to Master server to th panel, choose Servers--Add new server(and provide server info from the Slave server), then for a get access to creating, managing, deleting VMs, we provide 2 values from proxmox API section to the slave setup in left panel of main server Virtualizor:

*Proxmox API token creating*
<img width="1078" height="445" alt="image" src="https://github.com/user-attachments/assets/872218b7-6d9e-44bf-99bb-4c82678b29b4" />

Do the same with remain servers. Next step is adding LVM(multipath - pairs to all 3 nodes of cluster) storage, which have already setuped by other department(but you can imagine that the storage is just standard LVM, it doesnt matter in this case).
<img width="492" height="123" alt="image" src="https://github.com/user-attachments/assets/e1bf6cc1-79ee-46de-ac47-28311ac47db7" />

### Blesta - add 1st service
1) add package
Like we see earlier in this project, we have integration (Blesta to Virtualizor) module, so by module, which provide a lot of useful functions, we have to setup one test package based on that module. Firstly we must create our 1st purchase form for customers:
<img width="996" height="502" alt="image" src="https://github.com/user-attachments/assets/acd78a42-a140-4d1d-ac3f-f31c7f1b0189" />


Now, we can add package(with a specific options):
<img width="1137" height="487" alt="image" src="https://github.com/user-attachments/assets/a25c05b4-91f3-4a22-ad7f-35f05b6c33a6" />
Then, LogIn to the client Area and choose created package with a flexible options:
<img width="932" height="593" alt="image" src="https://github.com/user-attachments/assets/5ef37016-aab0-4ad9-9d57-84728023fdee" />
And cause we haven't right now working payment gateway, but we have also opportunity to accept service in the Staff Area:
<img width="922" height="493" alt="image" src="https://github.com/user-attachments/assets/9fe97974-d28a-44ae-8682-94d8c7727536" />


Bottom line: we created packages in blesta billing, made a flexible customizable template in the Virtualizor panel, and also added all the performance options for launching VMs. Below I will demonstrate the working logic of the creation of the machines itself, since the goal of the project was not a fully working model (including payment through the payment gateway of client services), but a clear idea of how much the automation and well-coordinated work of the two services will simplify routine tasks, and will allow administrators to occasionally Interfere with the process of configuring-creating-deploying-works. Thus, this project fulfilled the main goal: to show the architecture and logic of work, from which it will be possible to start in the future. The project did not greatly affect the aspects of security and fault tolerance at the deep level, because the factor was that it was specifically a demonstration stand, but it is from this that you can start doing great things further, since the project has room for growth, there is something to refine, to modify the logic and etc. 
Below I will show you a video of approximately the work without other improvements, but how it looks from the administrator's side, although it will affect the client-side parts at the beginning. 
Happy viewing!!! 

## 🎥 Infrastructure Demo

[[![Watch the video](docs/IMG_20260309_141920_830.jpg)](docs/VID_20260309_140302_161.mp4)](https://youtu.be/XYLq4wfqd-w)


