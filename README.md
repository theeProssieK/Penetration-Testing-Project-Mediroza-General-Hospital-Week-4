<div align="center">

# 🔐 Cybersecurity Lab Environment Setup

**An isolated virtual lab for penetration testing and ethical hacking practice**

![VirtualBox](https://img.shields.io/badge/Hypervisor-VirtualBox-183A61?style=for-the-badge&logo=virtualbox&logoColor=white)
![Kali Linux](https://img.shields.io/badge/OS-Kali%20Linux%202026.3-557C94?style=for-the-badge&logo=kalilinux&logoColor=white)
![Network](https://img.shields.io/badge/Network-10.0.0.0%2F24-2E8B57?style=for-the-badge)
![Host](https://img.shields.io/badge/Host-Windows%2011-0078D6?style=for-the-badge&logo=windows11&logoColor=white)
![Status](https://img.shields.io/badge/Status-Completed-success?style=for-the-badge)

</div>

---

## 📑 Table of Contents

- [Project Overview](#-project-overview)
- [Objectives](#-objectives)
- [Purpose of the Lab](#-purpose-of-the-lab)
- [Lab Architecture](#-lab-architecture)
- [Lab Configuration](#-lab-configuration)
- [Setup Procedure](#-setup-procedure)
- [Lab Verification](#-lab-verification)
- [Troubleshooting](#-troubleshooting)
- [What I Learned](#-what-i-learned)
- [Security and Ethical Use](#-security-and-ethical-use)
- [Tools and Resources](#-tools-and-resources)
- [Author](#-author)

---

## 📌 Project Overview

This project documents how I built a **virtual cybersecurity and penetration-testing laboratory** using **VirtualBox** and **Kali Linux**.

The goal is a controlled, repeatable environment where security tools, network scanning, reconnaissance and vulnerability assessment can be practised safely. The lab runs on a **private NAT Network**, so more virtual machines can be added later and used as targets for authorized testing.

---

## 🎯 Objectives

- Download and verify the Kali Linux virtual machine package.
- Install and configure VirtualBox.
- Extract and import Kali Linux as a virtual machine.
- Create a private NAT Network for the lab.
- Assign a static IP address to the Kali VM.
- Verify connectivity and DNS resolution.
- Take a clean VM snapshot for recovery.
- Document the full process for future projects.

---

## 🧪 Purpose of the Lab

The lab provides an isolated space for learning and authorized security testing, such as:

- 🔎 Network reconnaissance
- 📡 Port scanning
- 🛡️ Vulnerability assessment
- 📦 Packet analysis
- 🌐 **Important:** Only use this lab on systems you own or have written permission to test. Never use these tools against unauthorized systems. Web security testing
- 🧰 Security-tool experimentation

> ⚠️ It is important to note that this laboratory must only be used for systems that you own or have proper permission to test. Ensure not to use the lab or its tools to attack unauthorized systems.

---

## 🗺️ Lab Architecture

```text
                +--------------------------------------+
                |   Host PC (Windows 11, 16 GB RAM)    |
                |                                      |
                |   +------------------------------+   |
                |   |   VirtualBox                 |   |
                |   |                              |   |
                |   |   NAT Network: 10.0.0.0/24   |   |
                |   |   Gateway: 10.0.0.1          |   |
                |   |        |                     |   |
                |   |   +----+------------------+  |   |
                |   |   | Kali Linux 2026.3     |  |   |
                |   |   | 10.0.0.2/24           |  |   |
                |   |   +-----------------------+  |   |
                |   |   (future target VMs here)   |   |
                |   +------------------------------+   |
                +------------------|-------------------+
                                   |
                                Internet
```

---

## ⚙️ Lab Configuration

| 🧩 Component | ⚙️ Configuration |
|---|---|
| 🖥️ Host OS | Windows 11 |
| 🧠 Host RAM | 16 GB |
| ⚡ Processor | [Your CPU model, e.g. Intel Core i5] |
| 💾 VM Storage Location | `F:\VMs\Kali` |
| 🧰 Hypervisor | VirtualBox [your version, e.g. 7.2] |
| 🐉 Security OS | Kali Linux 2026.3 (pre-built VirtualBox image) |
| 🧠 Kali RAM | [4096 MB] |
| ⚙️ Kali CPUs | [2] |
| 🌐 Virtual Network | NAT Network (`NatNetwork`) |
| 📡 Network Address | 10.0.0.0/24 |
| 🐧 Kali IP Address | 10.0.0.2/24 |
| 🚪 Default Gateway | 10.0.0.1 |
| 🌍 DNS Server | 8.8.8.8 |

---

## 🛠️ Setup Procedure

### Step 1. Download the Kali Linux VM package

I downloaded the pre-built **Kali Linux VirtualBox image** (`kali-linux-2026.3-virtualbox-amd64.7z`) from the official Kali website: https://www.kali.org/get-kali/

### Step 2. Verify the download (SHA-256)

Before using the file, I compared its SHA-256 hash with the one published on the official site, to confirm the download was complete and untampered.

```powershell
Get-FileHash "C:\Users\<your-user>\Downloads\kali-linux-2026.3-virtualbox-amd64.7z" -Algorithm SHA256
```

The hash matched the published value. ✅

![SHA-256 verification](screenshots/01-hash-verification.png)

### Step 3. Install VirtualBox

I installed **Oracle VirtualBox** as the hypervisor and switched the interface to **Expert Mode** so that all network tools are visible.

### Step 4. Extract the Kali package

The package is a `.7z` archive, so I extracted it with **WinRAR** (7-Zip also works) to a drive with enough free space (about 25 GB):

```text
F:\VMs\Kali\kali-linux-2026.3-virtualbox-amd64\
```

### Step 5. Add the VM to VirtualBox

In VirtualBox: **Machine → Add…**, then select the `.vbox` file inside the extracted folder.

> The folder must stay in the same place afterwards, because VirtualBox runs the VM from there.

### Step 6. Create the NAT Network

In VirtualBox: **Tools → Network Manager → NAT Networks → Create**.

| Setting | Value |
|---|---|
| Name | NatNetwork |
| IPv4 Prefix | 10.0.0.0/24 |
| DHCP | Enabled |
| IPv6 | Disabled |

![NAT Network settings](screenshots/02-nat-network.png)

A **NAT Network** was chosen because VMs on the same NAT Network can talk to each other **and** reach the internet. This is ideal for adding attacker and target machines later.

### Step 7. Configure the Kali VM

With the VM powered off, open **Settings** and set:

| Section | Setting | Value |
|---|---|---|
| System → Motherboard | Base Memory | [4096 MB] |
| System → Processor | CPUs | [2] |
| Display | Video Memory | 128 MB |
| Network → Adapter 1 | Attached to | NAT Network |
| Network → Adapter 1 | Name | NatNetwork |

![VM network adapter](screenshots/03-vm-network-adapter.png)

### Step 8. Start Kali and log in

I started the VM from VirtualBox Manager and logged in with the default prebuilt-image credentials, then changed the default password with `passwd`.

![Kali desktop](screenshots/07-kali-desktop.png)

### Step 9. Configure a static IP address

The Kali Linux network configuration was checked and configured with a consistent IPv4 address.

Example configuration:

In Kali: **Network icon → Edit Connections → Wired connection 1 → IPv4 Settings**.

| Field | Value |
|---|---|
| Method | Manual |
| Address | 10.0.0.2 |
| Netmask | 24 (255.255.255.0) |
| Gateway | 10.0.0.1 |
| DNS servers | 8.8.8.8 |

A fixed address makes the lab easier to document and to reference in later exercises.

![Static IP configuration](screenshots/04-static-ip.png)

### Step 10. Create a clean VM snapshot

With the VM configured and working, I took a snapshot as a recovery point.

| Field | Value |
|---|---|
| Snapshot name | Kali - Clean Network Setup |
| Description | Fresh Kali 2026.3, NAT Network 10.0.0.0/24, static IP 10.0.0.2, no tools or updates added yet. |

If a later exercise breaks the VM, I can restore this baseline.

![Snapshot](screenshots/06-snapshot.png)

---

## ✅ Lab Verification

| ✅ Test | 🧾 Command | 🎯 Expected Result |
|---|---|---|
| 🌐 Check IP address | `ip a` | `10.0.0.2/24` shown on the network interface |
| 🛣️ Check routing | `ip route` | Default route via `10.0.0.1` |
| 📡 Test gateway | `ping -c 3 10.0.0.1` | Successful replies |
| 🌍 Test internet | `ping -c 3 8.8.8.8` | Successful replies |
| 🔎 Test DNS | `ping -c 3 google.com` | Name resolves and replies arrive |
| 🧰 Verify Nmap | `nmap --version` | Nmap version displayed |
| 🔄 Verify snapshot | Restore snapshot, then run `ip a` | Baseline configuration restored |

![Verification results](screenshots/05-verification.png)

### Results

```text
IP Address : 10.0.0.2/24
Gateway    : 10.0.0.1
DNS        : 8.8.8.8
Internet   : reachable
```

---

## 🧯 Troubleshooting

Problems I ran into and how I solved them:

| ❌ Problem | 💡 Cause | ✅ Fix |
|---|---|---|
| "File not found" when adding the VM | The file name box held a leftover default (`VirtualBox VMs`), and I had not opened the extracted subfolder | Open the extracted folder and select the `.vbox` file |
| VM does not appear in the list | I was looking at the Network Manager page instead of the Machines view | Click the **Machines** icon in the left column |
| "Machine is already locked by a session" | The VM was opened several times by double-clicking the `.vbox` file | Close the VM window, power it off, and start it only once from VirtualBox Manager |
| VM shows **Inaccessible** | The VM files were moved or deleted, but VirtualBox still listed it | Right-click it, choose **Remove → Remove Only**, then add the VM again |
| "Creating host-only network interface… canceled" | The Windows permission or driver prompt was dismissed | Click **Yes** on the prompt, or run VirtualBox as administrator; the default host-only adapter was not needed for this lab |
| VM screen looks grey and does not respond | The VM was paused | Use **Machine → Pause** (or Right Ctrl + P) to resume |

https://github.com/theeProssieK/Networkwalks-B084-week1-Cybersecurity-lab-setup
---

## 📚 What I Learned

### 1. NAT vs NAT Network
**NAT** lets a VM reach the internet through the host but does not allow direct VM-to-VM communication. A **NAT Network** lets several VMs share one private network while keeping internet access, which suits a multi-machine lab.

### 2. Virtual machine networking
VirtualBox network adapters connect VMs to different network types, and the adapter mode decides who can talk to whom.

### 3. Static IP configuration
I learned how addresses, netmasks, gateways and DNS work together, and how to configure and verify them in Kali.

### 4. File integrity
Checking a SHA-256 hash confirms a download is complete and genuine before it is used.

### 5. Snapshots
A clean snapshot is a known-good recovery point to return to before risky or experimental work.

### 6. Documentation
Recording commands, settings, screenshots, problems and fixes is a key part of professional cybersecurity work.

---

## 🛡️ Security and Ethical Use

This lab is intended **strictly for education and authorized testing**. Only test systems you own or have explicit permission to assess. The author accepts no responsibility for misuse.

---

## 🔗 Tools and Resources

- **VirtualBox:** https://www.virtualbox.org/wiki/Downloads
- **Kali Linux:** https://www.kali.org/get-kali/
- **WinRAR:** https://www.win-rar.com/
- **7-Zip (alternative):** https://www.7-zip.org/download.html
- **Git for Windows:** https://git-scm.com/

---

## 👤 Author

**[Your Full Name]**
Cybersecurity Student, [Program / Cohort Code]

- GitHub: [https://github.com/your-username](https://github.com/your-username)
- LinkedIn: [https://www.linkedin.com/in/your-profile](https://www.linkedin.com/in/your-profile)

### 📋 Project Information

| | |
|---|---|
| **Program** | [Program Name] |
| **Week** | [01] |
| **Project** | Cybersecurity and Pentesting Lab Setup |
| **Repository** | GitHub |

### 🙏 Acknowledgement

This project's structure was guided by the course reference material provided by [Instructor / Program Name].

---

<div align="center">

⭐ If you found this useful, feel free to star the repo.

</div>
