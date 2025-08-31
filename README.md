# BeEF-XSS
BeEF (Browser Exploitation Framework) is a penetration testing tool focusing on web browsers. It "hooks" a target’s browser
# 🧠 BeEF-XSS – Ethical Hacking & Browser Exploitation Lab

This project demonstrates how to use the **Browser Exploitation Framework (BeEF)** with **Cross-Site Scripting (XSS)** vulnerabilities to simulate client-side attacks in a controlled, ethical hacking environment.

> ⚠️ **Disclaimer**: This lab is intended for educational and authorized penetration testing purposes only. Do not use these techniques on systems you do not own or have explicit permission to test.

---

## 📚 Chapter 1: Setting Up the Virtual Lab

We create a safe, isolated environment for ethical hacking and browser exploitation:

### 🔧 Required Virtual Machines

- **Kali Linux** (Attacker Machine)
- **Ubuntu Desktop VMs** (Victim Machines)
- **Metasploitable2** (Vulnerable Linux Server)

### 🖥️ Tool: VirtualBox

VirtualBox is a free and open-source virtualization tool to run our VMs.

1. Download from [https://www.virtualbox.org/wiki/Downloads](https://www.virtualbox.org/wiki/Downloads)
2. Install and create virtual machines with proper specs.
3. Configure **networking**: Use "Internal Network" for isolated attack scenarios.

---

## 💿 Chapter 2: Installing Kali Linux

### ✅ Method 1: Dual Boot (Windows + Kali Linux)

- Use **Rufus** to create bootable USB.
- Shrink Windows partition (30–50GB).
- Install Kali from USB.
- Setup GRUB bootloader to dual boot between Windows and Kali.

### ✅ Method 2: VirtualBox (Recommended)

1. Download Kali VirtualBox image from [https://www.kali.org](https://www.kali.org)
2. Import `.ova` file into VirtualBox.
3. Set **network to Internal Network**.
4. Boot up Kali Linux VM.

> 💡 Fix sources with:
```bash
sudo nano /etc/apt/sources.list
# Add:
deb http://http.kali.org/kali kali-rolling main contrib non-free non-free-firmware
sudo apt update && sudo apt full-upgrade -y
