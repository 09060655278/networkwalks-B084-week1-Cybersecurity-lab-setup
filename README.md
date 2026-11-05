# 🔐 Cybersecurity Lab Environment Setup

**Building an isolated virtual lab with Kali Linux and VirtualBox for ethical hacking and penetration testing practice**

![Skill](https://img.shields.io/badge/Skill-Cybersecurity-404040?style=flat-square&labelColor=C00000)
![VirtualBox](https://img.shields.io/badge/Ver-VirtualBox%20v7.X-0070C0?style=flat-square&labelColor=000000&logo=virtualbox&logoColor=white)
![Kali](https://img.shields.io/badge/Kali%20Linux-v20XX.X-E87500?style=flat-square&labelColor=000000&logo=kalilinux&logoColor=white)
![Skill](https://img.shields.io/badge/Skill-Linux-404040?style=flat-square&labelColor=C00000)
![Network](https://img.shields.io/badge/Network-10.0.0.0%2F24-238F89?style=flat-square&labelColor=000000)
![Pentest](https://img.shields.io/badge/Penetration%20Testing-C00000?style=flat-square&labelColor=000000&logo=kalilinux&logoColor=white)
![Skill](https://img.shields.io/badge/Skill-Virtualization-404040?style=flat-square&labelColor=C00000)
![GitHub](https://img.shields.io/badge/GitHub-404040?style=flat-square&labelColor=0070C0&logo=github&logoColor=white)
![NetworkWalks](https://img.shields.io/badge/NetworkWalks-B084-404040?style=flat-square&labelColor=C00000)
![Ethical](https://img.shields.io/badge/Ethical%20Hacking-E87500?style=flat-square&labelColor=000000&logo=kalilinux&logoColor=white)

---

## 📌 Project Overview

This project sets up a **virtual cybersecurity and penetration-testing laboratory** using VirtualBox and Kali Linux on my {{Windows 10 / Windows 11 / Linux / macOS}} computer.

The lab is a controlled, isolated environment where I can safely and repeatedly practise network scanning, reconnaissance, vulnerability assessment, and other security-testing skills.

All machines sit on a **private virtual network**, so more machines can be added later as targets for authorized testing.

---

## 🎯 Objectives

The main objectives of this project are to:

- Install and configure VirtualBox.
- Install or import Kali Linux as a virtual machine.
- Create a private **NAT Network** for the lab.
- Give the Kali VM a consistent IP address.
- Verify network connectivity and DNS resolution.
- Take a clean snapshot for recovery.
- Document the whole process, including the problems I solved.
- Prepare the environment for future cybersecurity projects.

---

## 🛡️ Purpose of the Lab

The lab provides a safe space for cybersecurity learning and authorized security testing. It can be used for:

- Network reconnaissance
- Port scanning
- Vulnerability assessment
- Packet analysis
- Web security testing
- Security-tool experimentation

⚠️ **Important:** Use this lab only on systems that you own or have **explicit written permission** to test. Never use these tools against unauthorized systems.

---

## 🏗️ Lab Architecture

```text
Host computer ({{Windows 11}}, {{8 GB}} RAM)
└── VirtualBox {{7.X}}
    └── NAT Network "NatNetwork" (10.0.0.0/24) ──► Internet (through NAT)
        ├── Kali Linux   10.0.0.2    attacker machine
        └── Target VMs   10.0.0.3+   to be added in later projects
```

![Lab architecture](images/1-lab-architecture.png)

*Additional target machines can join the same virtual network in future projects.*

---

## ⚙️ Lab Configuration

| 🧩 Component | ⚙️ Configuration |
|---|---|
| 🖥️ Host OS | {{Windows 11}} |
| 🧠 Host RAM | {{8 GB}} |
| ⚡ Processor | {{Intel Core i5}} |
| 🧰 Hypervisor | VirtualBox {{7.X}} |
| 🐉 Security OS | Kali Linux {{20XX.X}} |
| 🧠 Kali RAM | {{2048 MB}} |
| 🔢 Kali CPU cores | {{2}} |
| 💾 Kali disk size | {{80 GB}} |
| 🌐 Virtual Network | NAT Network |
| 📡 Network Address | {{10.0.0.0/24}} |
| 🐧 Kali IP Address | {{10.0.0.2/24}} |
| 🚪 Default Gateway | {{10.0.0.1}} |
| 🌍 DNS Server | {{8.8.8.8}} |

---

# 🪜 Lab Setup Procedure

## Step 1. Check the computer and enable virtualization

The computer needs a 64-bit processor with hardware virtualization (Intel VT-x or AMD-V) turned on in the BIOS/UEFI. I also made sure I had enough free disk space ({{at least 60 GB}}) and RAM.

---

## Step 2. Install VirtualBox

I downloaded VirtualBox from the official website and installed it with the default options.

**Version installed:** {{7.X}}

---

## Step 3. Create the NAT Network

I created a dedicated NAT Network in VirtualBox (**Tools → Network → NAT Networks → Create**).

```text
Network Name:  NatNetwork
IPv4 Prefix:   10.0.0.0/24
DHCP:          Enabled
IPv6:          Disabled
```

![NAT Network settings](images/2-nat-network-settings.png)

A **NAT Network** was chosen because machines on the same NAT Network can talk to each other and still have outbound internet access. This lets attacker and target VMs communicate inside the lab.

---

## Step 4. Download and import Kali Linux

I downloaded the official Kali Linux virtual machine image from the Kali website, {{extracted it with 7-Zip because it comes as a .7z archive,}} and imported it into VirtualBox.

I then checked the VM settings:

```text
Adapter 1
Attached to:  NAT Network
Network:      NatNetwork

RAM:          {{2048 MB}}
CPU cores:    {{2}}
```

![Kali VM settings](images/3-kali-vm-settings.png)

{{Optional: a shared folder and shared clipboard were enabled for moving files between the host and the VM.}}

---

## Step 5. Configure the Kali network

I checked the network connection name and set a static IPv4 address so the machine is easy to reference in later exercises.

```bash
# 1. Find the connection name
nmcli connection show

# 2. Set a static address (replace "Wired connection 1" with your own connection name)
sudo nmcli connection modify "Wired connection 1" \
  ipv4.method manual \
  ipv4.addresses 10.0.0.2/24 \
  ipv4.gateway 10.0.0.1 \
  ipv4.dns 8.8.8.8

# 3. Apply it
sudo nmcli connection up "Wired connection 1"
```

Configuration used:

```text
IP Address:   10.0.0.2
Subnet Mask:  255.255.255.0
Gateway:      10.0.0.1
DNS:          8.8.8.8
```

![Kali IP configuration](images/4-kali-ip-config.png)

> The address was chosen outside the range that DHCP hands out, so two machines never share the same IP.

---

## Step 6. Update Kali and secure the account

```bash
sudo apt update && sudo apt full-upgrade -y
passwd
```

The pre-built Kali VM ships with a well-known default password, so I changed it straight away.

---

## Step 7. Create a clean snapshot

After finishing the setup, I took a VirtualBox snapshot named:

```text
Clean Kali - Network Setup
```

![Snapshot](images/6-snapshot.png)

If a later experiment breaks the VM, I can restore this known-good baseline.

---

# 🔎 Lab Verification

| ✅ Test | 🧾 Command | 🎯 Expected Result |
|---|---|---|
| 🌐 Check IP address | `ip a` | Correct Kali IP displayed |
| 📡 Test gateway | `ping -c 4 10.0.0.1` | Successful replies |
| 🌍 Test internet | `ping -c 4 8.8.8.8` | Successful replies |
| 🔎 Test DNS | `nslookup kali.org` | Domain resolves to an address |
| 🧰 Verify Nmap | `nmap --version` | Nmap version displayed |
| 🔄 Verify snapshot | Restore snapshot, then `ip a` | Baseline configuration restored |

![Connectivity tests](images/5-connectivity-tests.png)

### Example results

```text
IP Address:  10.0.0.2/24
Gateway:     10.0.0.1  (reachable)
DNS:         resolving correctly
Internet:    reachable
```

---

# 🐞 Problems Encountered & Solutions

> {{Keep only the problems you really met, and add your own. Honest troubleshooting notes are the most valuable part of a lab write-up.}}

## Problem 1. VirtualBox VT-x / virtualization error

The VM would not start because hardware virtualization was turned off in the BIOS/UEFI.

**Solution:**

1. Restart the computer and open the BIOS/UEFI settings.
2. Enable Intel VT-x (or AMD-V) / hardware virtualization.
3. Save and restart.
4. Start the Kali VM again.

---

## Problem 2. No internet after setting a static IP

After setting the address by hand, the VM could reach the gateway but not the internet.

**Solution:** I checked the gateway and DNS values, then restarted the networking service and tested again:

```bash
sudo systemctl restart NetworkManager
ping -c 4 8.8.8.8
```

> Connection names differ between systems, so find yours with `nmcli connection show` first.

---

## Problem 3. The NAT Network was not available on the VM

The network adapter showed no network to choose from.

**Solution:** The NAT Network has to be created in VirtualBox **before** the VM can use it. After I created it (Step 3), it appeared in the adapter list.

---

## Problem 4. {{Your own problem}}

{{What happened, and how you fixed it.}}

---

# 💡 What I Learned

> {{Rewrite this section in your own words, using what really surprised or taught you.}}

### 1. NAT vs NAT Network

A plain NAT gives each VM its own private connection, so VMs cannot talk to each other. A **NAT Network** lets VMs on the same network communicate while still reaching the internet, which is what a multi-machine lab needs.

### 2. Virtual machine networking

Each VM network adapter can be attached to a different kind of network, and that choice decides which machines can see each other.

### 3. Static IP configuration

I learned to set and verify the IPv4 address, subnet mask, gateway, and DNS server in Kali Linux.

### 4. Snapshots

A clean snapshot should be taken **before** any risky or experimental activity. It gives me a known-good point to return to.

### 5. Documentation

Writing down commands, settings, screenshots, and problems is a core part of professional security work.

---

# 🔐 Security & Ethical Use

This laboratory is for **education only**.

- Test only systems you own or have written permission to test.
- Keep lab machines on the private virtual network.
- Do not use these tools or skills against schools, businesses, or other people.
- Unauthorized access to computer systems is a crime in most countries.

---

# 🔗 Tools & Resources

- **VirtualBox:** https://www.virtualbox.org/wiki/Downloads
- **Kali Linux:** https://www.kali.org/get-kali/
- **7-Zip:** https://7-zip.org/download.html

---

# 👤 Author

**AKANBI TEMITOPE BENJAMEN**
Cybersecurity at NetworkWalks, Batch B084

LinkedIn: https://www.linkedin.com/in/temitope-akanbi-749363442/
GitHub: {{your GitHub link}}

---

## 📌 Project Information

**Program Name:** Cybersecurity at NetworkWalks | **Week:** 01 | **Project:** Cybersecurity & Pentesting Lab Setup | **Repository:** GitHub
