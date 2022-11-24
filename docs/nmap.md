---
title: Nmap
date: 2022-11-21 09:42:24
icon: icon-nmap
background: bg-cyan-300
tags:
  - recon
  - port scan
  - vul scan
categories:
    - Reconnaissance
intro: |
      The nmap cheat sheet quick reference of most commonly used shortcuts and commands
---



Nmap Commands {.cols-1}
-------

### Nmap shortcuts

Quick Combinations of Commands

```shell script
$ ports=$(nmap -p- --min-rate=1000  -T4 <IP> | grep ^[0-9] | cut -d '/' -f1 | tr '\n' ',' | sed s/,$//)
$ nmap -p$ports -sC -sV <IP> | tee nmap/portsall.txt
```

Quick Scanning TCP and UDP Open port

```shell script
$ nmap --min-rate=2000 --max-retries=0 -sT -sU -4 <IP> -Pn > portscan
```

Quick Scanning UDP Open port

```shell script
$ nmap -sU -sV -O -p- -Pn -vv -oA nmap/udp <IP>
$ nmap -sU --top-ports 10 -sV -oA nmap/udp <IP>
```

Quick Scanning Default NSE scripts. Considered useful for discovery and safe

```shell script
$ nmap -p- -n -Pn -sC -sV --min-rate 2000 -oA nmap/portscan -v <IP>
```

Dump scan logs using the tee command

```shell script
$ nmap -p- --min-rate 10000 <IP> | tee scans/nmap-alltcp.txt
$ nmap --script vuln -sV -p- -Pn <IP> | tee nmap/vuln.txt
```

Example IDS Evasion command

```shell script
$ nmap -f -t 0 -n -Pn –data-length 200 -D 192.168.1.101,192.168.1.102,192.168.1.103,192.168.1.23 192.168.1.1
```

Nmap help {.cols-3}
-------

### Basic Scanning Techniques {.row-span-1}

The `-s` switch determines the type of scan to perform.

| Nmap Switch | Description                 |
|:------------|:----------------------------|
| **-sA**     | ACK scan                    |
| **-sF**     | FIN scan                    |
| **-sI**     | IDLE scan                   |
| **-sL**     | DNS scan (a.k.a. list scan) |
| **-sN**     | NULL scan                   |
| **-sO**     | Protocol scan               |
| **-sP**     | Ping scan                   |
| **-sR**     | RPC scan                    |
| **-sS**     | SYN scan                    |
| **-sT**     | TCP connect scan            |
| **-sW**     | Windows scan                |
| **-sX**     | XMAS scan                   |

### Port Scanning Technology

| Nmap Switch     | Description                                                           |
|:----------------|:----------------------------------------------------------------------|
| **-p**          | Port scan for port x                                                  |
| **-p**          | Port range                                                            |
| **-p**          | Port scan multiple TCP and UDP ports                                  |
| **-p-**         | Port scan all ports                                                   |
| **-p**          | Port scan from service name                                           |
| **-F**          | Fast port scan (100 ports)                                            |
| **--top-ports** | Port scan the top x ports                                             |
| **-p-65535**    | Leaving off initial port in range<br />makes the scan start at port 1 |
| **-p0-**        | Leaving off end port in range makes the scan go through to port 65535 |

### Service and Version Detection

| Nmap Switch                 | Description                                                                |
|:----------------------------|:---------------------------------------------------------------------------|
| **-sV**                     | Attempts to determine the version of the service running on port           |
| **-sV --version-intensity** | Intensity level 0 to 9. Higher number increases possibility of correctness |
| **-sV --version-light**     | Enable light mode. Lower possibility of correctness. Faster                |
| **-sV --version-all**       | Enable intensity level 9. Higher possibility of correctness. Slower        |
| **-A**                      | Enables OS detection, version detection, script scanning, and traceroute   |

### Additional Scanning Technology

| Nmap Switch | Description               |
|:------------|:--------------------------|
| **-iL**        | Scan targets from a file  |
| **-iR**        | Scan 100 random hosts |
| **--exclude**  | Exclude listed hosts   |

### OS Detection

| Nmap Switch           | Description                                                               |
|:----------------------|:--------------------------------------------------------------------------|
| **-O**                | stack fingerprinting                                                      |
| **-O --osscan-limit** | OS detection against host                                                 |
| **-O --osscan-guess** | Makes Nmap guess more aggressively                                        |
| **-O --max-os-tries** | detection tries against a target                                          |
| **-A**                | Enables OS detection, version detection, script scanning, and traceroute  |

### Output {.row-span-2}

| Switch                                                         | Description                                                                                     |
|:---------------------------------------------------------------|:------------------------------------------------------------------------------------------------|
| **-oN**                                                        | Normal output to the file normal.file                                                           |
| **-oX**                                                        | XML output to the file xml.file                                                                 |
| **-oG**                                                        | Grepable output to the file grep.file                                                           |
| **-oA**                                                        | Output in the three major formats at once                                                       |
| **-oG -**                                                      | Grepable output to screen. -oN -, -oX - also usable                                             |
| **--append-output**                                            | Append a scan to a previous scan file                                                           |
| **-v**                                                         | Increase the verbosity level (use -vv or more for greater effect)                               |
| **-d**                                                         | Increase debugging level (use -dd or more for greater effect)                                   |
| **--reason**                                                   | Display the reason a port is in a particular state, same output as -vv                          |
| **--open**                                                     | Only show open (or possibly open) ports                                                         |
| **--packet-trace**                                             | Show all packets sent and received                                                              |
| **--iflist**                                                   | Shows the host interfaces and routes                                                            |
| **--resume**                                                   | Resume a scan                                                                                   |


### Timing and Performance {.row-span-2}

| Nmap Switch                     | Description                                                                                            |
|:--------------------------------|:-------------------------------------------------------------------------------------------------------|
| **-T0**                         | Paranoid (0) Intrusion Detection<br />System evasion                                                   |
| **-T1**                         | Sneaky (1) Intrusion Detection System <br />evasion                                                    |
| **-T2**                         | Polite (2) slows down the scan to use <br />less bandwidth and use less target <br />machine resources |
| **-T3**                         | Normal (3) which is default speed                                                                      |
| **-T4**                         | Aggressive (4) speeds scans; assumes <br />you are on a reasonably fast and <br />reliable network     |
| **-T5**                         | Insane (5) speeds scan; assumes you <br />are on an extraordinarily fast network                       |
| **--max-retries** &lt;tries&gt; | Specify the maximum number <br />of port scan probe retransmissions                                    |
| **--min-rate** &lt;numberr&gt;  | Send packets no slower than &lt;numberr&gt; per second                                                 |
| **--max-rate** &lt;numberr&gt;  | Send packets no faster than &lt;number&gt; per second                                                  |

### Firewall / IDS Evasion and Spoofing

| Switch                                                         | Description                                                                                     |
|:---------------------------------------------------------------|:------------------------------------------------------------------------------------------------|
| **-f**                                                             | Requested scan (including ping scans) use tiny fragmented IP packets. Harder for packet filters |
| **--mtu**                                                          | Set your own offset size                                                                        |
| **-D**                                                             | Send scans from spoofed IPs                                                                     |
| **-S**                                                             | Scan Facebook from Microsoft (-e eth0 -Pn may be required)                                      |                  
| **-g**                                                             | Use given source port number                                                                    |
| **--proxies**                                                      | Relay connections through HTTP/SOCKS4 proxies                                                   |
| **--data-length**                                                  | Appends random data to sent packets                                                             |


### NSE Scripts

| Nmap Switch    | Description        |
|:--------------|:-----------|
| **-sC**  | Scan with default NSE scripts. Considered useful for discovery and safe   |
| **--script default**   | Scan with default NSE scripts. Considered useful for discovery and safe   |

## Examples

### Useful NSE Script Examples

| Command                                                                                                                  | Description  |
|--------------------------------------------------------------------------------------------------------------------------|-----|
| `$ nmap -Pn --script=http-sitemap-generator scanme.nmap.org`                                                             |  http site map generator|
| `$ nmap -n -Pn -p 80 --open -sV -vvv --script banner,http-title -iR 1000`                                                |  Fast search for random web servers |
| `$ nmap -Pn --script=dns-brute domain.com`                                                                               |  Brute forces DNS hostnames guessing subdomains|
| `$ nmap -n -Pn -vv -O -sV --script smb-enum*,smb-ls,smb-mbenum,smb-os-discovery,smb-s*,smb-vuln*,smbv2* -vv 192.168.1.1` |  Safe SMB scripts to run |
| `$ nmap --script whois* domain.com`                                                                                      |  Whois query|
| `$ nmap -p80 --script http-unsafe-output-escaping scanme.nmap.org`                                                       |  Detect cross site scripting vulnerabilities |
| `$ nmap -p80 --script http-sql-injection scanme.nmap.org`                                                                                                                     |  Check for SQL injections|


### Helpful Nmap Output examples

Scan for web servers and grep to show which IPs are running web servers
```shell script
$ nmap -p80 -sV -oG - --open 192.168.1.1/24  | grep open
```

Generate a list of the IPs of live hosts 

```shell script
$ nmap -iR 10 -n -oX out.xml | grep "Nmap" | cut -d " " -f5 > live-hosts.txt
```

Reverse sorted list of how often ports turn up

```shell script
$  grep " open " results.nmap | sed -r 's/ +/ /g' | sort | uniq -c | sort -rn | less 
```

### Other Useful Nmap Commands

| Command                                               | Description                                         |
|:------------------------------------------------------|:----------------------------------------------------|
| `$ nmap -iR 10 -PS22-25,80,113,1050,35000 -v -sn`     | Discovery only on ports x, no port scan             |
| `$ nmap 192.168.1.1-1/24 -PR -sn -vv`                 | Arp discovery only on local network, no port scan   |
| `$ nmap -iR 10 -sn -traceroute`                       | Traceroute to random targets, no port scan          |
| `$ nmap 192.168.1.1-50 -sL --dns-server 192.168.1.1`  | Query the Internal DNS for hosts, list targets only |


Also see
-------

- [Nmap Docs](https://nmap.org/docs.html)
- [AutoRecon Script](https://github.com/Tib3rius/AutoRecon)