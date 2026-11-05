# 🔐 CYBERSECURITY LAB ENVIRONMENT SETUP
### Building a Virtual Cybersecurity Lab Using Kali Linux & Oracle VirtualBox

<p align="center">
  <img src="https://img.shields.io/badge/Platform-Kali%20Linux-blue?style=for-the-badge&logo=kalilinux" />
  <img src="https://img.shields.io/badge/Virtualization-VirtualBox-darkblue?style=for-the-badge&logo=virtualbox" />
  <img src="https://img.shields.io/badge/Focus-Cybersecurity-red?style=for-the-badge" />
  <img src="https://img.shields.io/badge/Status-In%20Progress-orange?style=for-the-badge" />
</p>

---

## 📌 PROJECT OVERVIEW

This project documents the process of building and configuring a virtual cybersecurity laboratory using **Kali Linux and Oracle VirtualBox**.

The primary goal is to create a controlled virtual environment for learning cybersecurity concepts, understanding Linux networking, exploring security tools, and developing practical technical skills.

The laboratory provides a foundation for future cybersecurity experiments, network analysis, system administration, and authorized security assessments.

This project also documents the installation process, network configuration, troubleshooting activities, and lessons learned throughout the setup.

---

## 🎯 PROJECT OBJECTIVES

The main objectives of this project are:

- Install and configure Oracle VirtualBox.
- Download and install Kali Linux.
- Configure Kali Linux as a virtual machine.
- Understand virtual machine networking.
- Configure and inspect network interfaces.
- Test network connectivity.
- Understand IP addressing and DNS.
- Troubleshoot network connectivity issues.
- Document the entire laboratory setup.
- Prepare the environment for future cybersecurity learning.

---

## 🛠️ TOOLS & TECHNOLOGIES

| Tool | Purpose |
|---|---|
| Kali Linux | Linux operating system for cybersecurity learning |
| Oracle VirtualBox | Virtualization software |
| Windows | Host operating system |
| Terminal | Executing Linux commands |
| NetworkManager | Managing network connections |
| GitHub | Project documentation and version control |

---

## ⚙️ LAB ENVIRONMENT CONFIGURATION

The following table describes the laboratory environment.

| Component | Configuration |
|---|---|
| Host Operating System | Windows |
| Virtualization Platform | Oracle VirtualBox |
| Guest Operating System | Kali Linux |
| Virtual Machine | Kali Linux VM |
| Network Management | NetworkManager |
| Network Interface | Ethernet |
| Network Configuration | VirtualBox Virtual Network |
| Project Type | Cybersecurity Home Lab |

> Note: Hardware specifications, IP addresses, and network settings will be updated as the laboratory configuration progresses.

---

# 🪜 LAB SETUP PROCEDURE

## STEP 1: Install Oracle VirtualBox

Oracle VirtualBox was installed to provide a virtualization environment for running Kali Linux without replacing the host operating system.

The software allows the creation and management of virtual machines.

## STEP 2: Download and Install Kali Linux

Kali Linux was obtained from its official distribution source and configured as a virtual machine inside VirtualBox.

The virtual machine was configured with the necessary resources to run the operating system.

## STEP 3: Configure the Virtual Machine

After installation, the Kali Linux virtual machine was started and its basic settings were checked.

The configuration included:

- Virtual machine memory allocation.
- Processor configuration.
- Network adapter settings.
- Storage configuration.
- Operating system initialization.

## STEP 4: Initial Kali Linux Configuration

After successfully booting Kali Linux, the initial system configuration was completed.

The following activities were performed:

- Logging into the Kali Linux desktop.
- Exploring the Linux environment.
- Opening the terminal.
- Checking available network interfaces.
- Inspecting network connection status.

## STEP 5: Network Interface Verification

The following command was used to inspect the network interfaces and their connection status.

```bash
nmcli device status
