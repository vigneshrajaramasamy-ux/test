# 🔐 My Cybersecurity Lab Setup (VirtualBox + Kali Linux)

**A small, isolated virtual lab I built to practise ethical hacking and penetration testing safely**

![Skill](https://img.shields.io/badge/Skill-Cybersecurity-404040?style=flat-square&labelColor=C00000)
![VirtualBox](https://img.shields.io/badge/VirtualBox-7.x-0070C0?style=flat-square&labelColor=000000)
![Kali](https://img.shields.io/badge/Kali%20Linux-2026.2-E87500?style=flat-square&labelColor=000000&logo=kalilinux&logoColor=white)
![Network](https://img.shields.io/badge/Network-10.0.0.0%2F24-238F89?style=flat-square&labelColor=000000)
![NetworkWalks](https://img.shields.io/badge/NetworkWalks-404040?style=flat-square&labelColor=C00000)

---

## 📌 About this project

This is my Week 1 task from the Networkwalks cybersecurity course (WK1-PM1). The job was simple to say but took a bit of effort to get right: set up a proper cybersecurity testing lab on my own laptop.

I used VirtualBox as the base and installed Kali Linux as my attacking machine. Everything sits on its own private network, so I can break things, scan things and experiment without touching anything real. Later on I plan to add Windows and Android machines to the same network and use them as targets.

I'm a beginner in cybersecurity, so I wrote this up mainly to keep a record for myself, but I hope it helps anyone doing the same task.

---

## 🎯 What the task asked for

- Use VirtualBox as the base (latest recommended version)
- Set up Kali Linux as the attacker machine
- Build the network in the `10.0.0.0/24` subnet using a **NAT Network**
- Turn on clipboard sharing and drag and drop in the VM settings
- Share the host `Downloads` folder with the VM
- Give Kali the IP address `10.0.0.2/24`
- Make sure Kali has full internet access

I ticked all of these off. The steps below show how.

---

## 🏗️ Lab architecture

![Lab architecture](lab-architecture.png)

Kali was built in Phase 1. The other machines in the diagram were optional, and they are now done too (Phase 2).

| Machine | IP address | Status |
| ------- | ---------- | ------ |
| Kali Linux (attacker) | 10.0.0.2/24 | ✅ Done |
| Windows 11 | 10.0.0.11/24 | ✅ Done |
| Windows 10 | 10.0.0.10/24 | ✅ Done |
| Windows 7 | 10.0.0.7/24 | ✅ Done |
| Windows Server 2016 | 10.0.0.16/24 | ✅ Done |
| Android 9 | 10.0.0.9/24 | ✅ Done |

---

## ⚙️ My configuration

| Item | What I used |
| ---- | ----------- |
| Host OS | Windows 11 |
| Host RAM | 32 GB |
| Processor | Intel Core i5 |
| Hypervisor | VirtualBox 7.x |
| Attacker OS | Kali Linux 2026.2 |
| Kali RAM | 2048 MB |
| Network type | NAT Network |
| Network range | 10.0.0.0/24 |
| Kali IP | 10.0.0.2/24 |
| Gateway | 10.0.0.1 |
| DNS | 8.8.8.8 |

---

# 🪜 Setup steps

## Step 1. Install 7-Zip

Kali's VirtualBox image comes as a compressed archive, so I needed 7-Zip to open it first.

Download: <https://7-zip.org/download.html>

## Step 2. Install VirtualBox

I installed the current recommended version of VirtualBox on my laptop. The default options were fine.

Download: <https://virtualbox.org/wiki/Downloads>

## Step 3. Create the NAT Network

In VirtualBox I went to **File → Tools → Network → NAT Networks** and created a new one with these values:

```
Name:        NatNetwork
IPv4 Prefix: 10.0.0.0/24
DHCP:        Enabled
IPv6:        Disabled
```

![NAT Network settings](nat-network-settings.png)

I picked a NAT Network (and not plain NAT) because VMs on the same NAT Network can talk to each other and still reach the internet. That is exactly what a lab with an attacker and several targets needs.

## Step 4. Download and import Kali Linux

I downloaded the prebuilt VirtualBox image from the official Kali site, extracted it with 7-Zip and opened the `.vbox` file in VirtualBox.

Download: <https://kali.org/get-kali>

Then I changed these VM settings:

```
Network  → Adapter 1
           Attached to: NAT Network
           Name:        NatNetwork
           Adapter type: Intel PRO/1000 MT Desktop
```

I also did the following in the VM settings:

- **General → Advanced:** set Shared Clipboard and Drag'n'Drop to Bidirectional
- **Shared Folders:** added my host `Downloads` folder, with Auto-mount turned on and full access
- **Acceleration:** left hardware virtualization and nested paging enabled

![Kali VM settings](3-screenshot-kali-linux.png)

## Step 5. Set Kali's IP address

Inside Kali I opened the network icon, chose **Edit Connections**, picked the wired connection and went to the IPv4 tab. I switched the method to **Manual** and entered:

```
Address:  10.0.0.2
Netmask:  24
Gateway:  10.0.0.1
DNS:      8.8.8.8
```

If the internet misbehaves, the course notes say you can try `10.0.0.1` as the DNS server instead.

![Kali network settings](4-screenshot-kali-network-settings.png)

## Step 6. Take a snapshot

Once everything worked, I took a VirtualBox snapshot so I always have a clean copy to go back to.

```
Snapshot name: Clean Kali - Network Setup
```

---

# 🔎 Checking that it all works

I ran these from the Kali terminal:

| What I tested | Command | What I expect |
| ------------- | ------- | ------------- |
| My IP address | `ip a` | Shows 10.0.0.2/24 |
| The gateway | `ping 10.0.0.1` | Replies come back |
| Internet access | `ping 8.8.8.8` | Replies come back |
| DNS | `nslookup networkwalks.com` | Name resolves to an IP |
| Shared folder | open the shared Downloads folder | Host files are visible |
| Clipboard | copy text on host, paste in Kali | Text pastes correctly |

---

# 💡 What I learned

- **NAT vs NAT Network:** plain NAT keeps each VM isolated from the others, while a NAT Network lets them see each other. I only really understood this after building it.
- **Static IPs:** setting the address, netmask, gateway and DNS by hand showed me how each piece fits together.
- **Snapshots:** take one before doing anything risky. It saves a lot of time.
- **Writing things down:** while documenting, I noticed small steps I would have forgotten.

---

# 🚀 What's next (Phase 2)

- Install Windows 11, 10 and 7 as target VMs
- Give each one its fixed IP from the table above
- Ping test every machine from every other machine
- Take a snapshot of each VM

---

# ⚠️ Ethical use

This lab is only for learning. I will only test machines that I built myself inside this lab, or ones I have clear written permission to test. Never use these tools against systems you do not own.

---

# 🔗 Links

- 7-Zip: <https://7-zip.org/download.html>
- VirtualBox: <https://virtualbox.org/wiki/Downloads>
- Kali Linux: <https://kali.org/get-kali>

---

# 👤 Author

**Vigneshraja Ramasamy**
LinkedIn: <https://www.linkedin.com/in/vignesh-raja-ramasamy-84123168>

**Program:** Cybersecurity at Networkwalks | **Week:** 01 | **Task:** WK1-PM1 Cybersecurity Lab Setup
