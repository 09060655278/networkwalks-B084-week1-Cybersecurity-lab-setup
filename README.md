# networkwalks-B084-week1-Cybersecurity-lab-setup
Week 1 of NetworkWalks (Batch B084): documentation of an isolated cybersecurity home lab with Kali Linux and target VMs, including network setup, verification steps and troubleshooting notes. 

# Cybersecurity Lab Setup (Week 1)

**NetworkWalks | Batch B084**

An isolated home cybersecurity lab built for Week 1 of the NetworkWalks programme. It documents how the lab was built, how it was tested, and what went wrong along the way.

> **Ethical use only.** Everything here is for learning inside an isolated lab you own. Never scan, test, or attack systems or networks without written permission.

---

## Table of contents
- [Overview](#overview)
- [Objectives](#objectives)
- [Lab architecture](#lab-architecture)
- [Requirements](#requirements)
- [Setup steps](#setup-steps)
- [Verification checklist](#verification-checklist)
- [Screenshots](#screenshots)
- [Troubleshooting](#troubleshooting)
- [Safety and ethics](#safety-and-ethics)
- [Repository structure](#repository-structure)
- [What I learned](#what-i-learned)
- [Next steps](#next-steps)
- [Author](#author)

---

## Overview
This repository records my Week 1 assignment: building a safe, isolated environment for practising cybersecurity skills. The lab has an attacker machine and [one or more] intentionally vulnerable target machines, all on a private virtual network that is not exposed to the internet or my home network.

## Objectives
- [ ] Install and configure a hypervisor ([VirtualBox / VMware Workstation / other])
- [ ] Build an attacker machine ([Kali Linux version])
- [ ] Build target machine(s) ([e.g. Metasploitable 2 / Windows / Ubuntu Server])
- [ ] Put all machines on an isolated virtual network
- [ ] Confirm the machines can talk to each other, and only to each other
- [ ] Take clean snapshots to roll back to
- [ ] Document the process and the problems I solved

## Lab architecture

```text
Host computer  ([OS], [RAM] GB RAM, [CPU], [disk] GB free)
└── Hypervisor ([name and version])
    ├── Kali Linux            (attacker)   ─┐
    ├── [Target VM 1]         (target)      ├── Isolated network: [Host-only / NAT Network / Internal]
    └── [Target VM 2]         (optional)   ─┘   Subnet: [192.168.56.0/24]
```

| Machine | OS and version | Role | RAM | CPU | Disk | Network | IP address |
|---|---|---|---|---|---|---|---|
| kali-attacker | Kali Linux [2025.x] | Attacker | [4 GB] | [2] | [60 GB] | [Host-only] | [192.168.56.10] |
| [target-1] | [Metasploitable 2] | Target | [1 GB] | [1] | [8 GB] | [Host-only] | [192.168.56.20] |

## Requirements

**Host computer**
- 64-bit CPU with virtualization turned on (Intel VT-x or AMD-V) in the BIOS or UEFI
- At least 8 GB RAM (16 GB is more comfortable) and about 100 GB of free disk space
- A stable internet connection for the downloads

**Software**
- Hypervisor: [VirtualBox / VMware Workstation Player / other]
- Attacker image: [Kali Linux, from the official kali.org site]
- Target image(s): [list them and where they came from]

> Download images only from the official source, and check the SHA-256 checksum before using them.

## Setup steps

### 1. Prepare the host
1. Turn on virtualization in the BIOS or UEFI.
2. Install the hypervisor and any extension pack it needs.
3. Create a folder for the VMs on a drive with enough space.

### 2. Download and verify the images
On Windows (PowerShell):
```powershell
Get-FileHash .\kali-linux-[version]-installer-amd64.iso -Algorithm SHA256
```
On Linux or macOS:
```bash
sha256sum kali-linux-[version]-installer-amd64.iso
```
Compare the result with the checksum published on the official download page.

### 3. Build the attacker VM
1. Create a new VM: Type **Linux**, Version **Debian (64-bit)**.
2. Give it [4 GB RAM], [2 CPUs], and [60 GB] disk.
3. Attach the ISO, install, and create a normal user. Do not leave the default credentials.
4. After installing, update the system:
```bash
sudo apt update && sudo apt full-upgrade -y
```
5. Install guest tools if you want copy and paste and a resizable screen: [command or steps].

### 4. Build the target VM(s)
[Describe how you imported or installed each target. For a ready-made VM, note the file type, such as .ova or .vmdk, and how you imported it.]

### 5. Configure the isolated network
1. Open the network settings of **each** VM.
2. Set adapter 1 to **[Host-only / NAT Network / Internal Network]** and choose the **same** network for every lab VM.
3. Make sure no vulnerable VM uses **Bridged** mode. A bridged vulnerable machine sits on your real network.
4. Note each machine's IP address:
```bash
ip a
```

### 6. Take snapshots
Take a snapshot of every VM in its clean state: **"Clean install, Week 1"**. Roll back to it whenever a machine breaks or after risky experiments.

## Verification checklist
- [ ] Attacker can ping each target: `ping -c 4 [target-ip]`
- [ ] Targets can ping the attacker
- [ ] A host discovery scan finds only my lab machines (run it only on your own lab subnet): `nmap -sn [192.168.56.0/24]`
- [ ] Vulnerable VMs **cannot** reach the internet or my home network
- [ ] The attacker can still update itself, if that is how I set it up
- [ ] Clean snapshots exist for all VMs
- [ ] Screenshots are saved in the `screenshots/` folder

## Screenshots

| What it shows | File |
|---|---|
| Hypervisor with all VMs | `screenshots/01-vm-list.png` |
| Network settings of each VM | `screenshots/02-network-settings.png` |
| `ip a` on the attacker | `screenshots/03-attacker-ip.png` |
| Successful ping to the target | `screenshots/04-ping-test.png` |
| Snapshots | `screenshots/05-snapshots.png` |

> Check each screenshot before you commit it. Blur anything personal, such as usernames, licence keys, or your real network details.

## Troubleshooting

| Problem | Cause | Fix |
|---|---|---|
| VM will not start, error about VT-x/AMD-V | Virtualization is off in the BIOS | Turn it on, and disable conflicting features such as Hyper-V if your hypervisor needs that |
| VMs cannot ping each other | They are on different networks or adapter types | Put every lab VM on the same host-only or NAT network |
| Target has no IP address | DHCP is not enabled on the network | [Enable DHCP on the host-only network or set a static IP] |
| Screen is tiny or copy and paste fails | Guest tools are missing | Install the guest additions or tools |
| Not enough memory | Too many VMs at once | Lower the VM RAM or run fewer machines |
| [Your own problem] | [Cause] | [How you fixed it] |

## Safety and ethics
- Keep vulnerable machines on an isolated network, never bridged to your real one.
- Practise only on machines you own or have written permission to test.
- Do not use these skills against other people, schools, or businesses.
- Computer misuse is illegal in most countries, and ignorance is not a defence.

## Repository structure

```text
.
├── README.md
├── .gitignore
├── docs/                 # step-by-step notes
├── screenshots/          # proof the lab works
├── configs/              # network or VM settings, exported
└── scripts/              # any helper scripts
```

## What I learned
- [Write 3 to 5 honest points, for example: how host-only networking works, why snapshots matter, how I fixed my biggest problem.]

## Next steps
- [ ] [Week 2 task]
- [ ] [Add a Windows target]
- [ ] [Practise basic scanning inside the lab]

## Author
AKANBI TEMITOPE
NetworkWalks, Batch B084
https://github.com/09060655278 | https://www.linkedin.com/in/temitope-akanbi-749363442/

## Licence
[MIT / or "For learning purposes only"]
