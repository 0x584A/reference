---
title: Masscan
date: 2022-11-22 11:28:03
icon: icon-nmap
background: bg-sky-100
tags:
  - recon
  - port scan
categories:
    - Reconnaissance
intro: |
      The Masscan cheat sheet quick reference of most commonly used shortcuts and commands
---



Usage and Options {.cols-2}
-------

### Timing and Performance {.row-span-3}

Used to scan in offline mode <br />
It does not send any traffic but just estimate the time length
```shell script
$ masscan 0.0.0.0/24 --offline
```

Using the rate to send X packets per second
```shell script
$ masscan 10.0.0.1/24 --rate 10000
```

Get banners from services (only few protocols supported)

```shell script
$ masscan 10.0.0.1 --banners
```

Assign masscan to another IP
```shell
$ masscan 10.0.0.1 --source-ip 192.168.1.200
```


Include a ping
```shell
$ masscan 10.0.0.1 --ping
```


Change the default user agent
```shell
$ masscan 10.0.0.1 --http-user-agent <user-agent>
```

Report only open ports
```shell
$ masscan 10.0.0.1 --open-only
```

Save sent packet in PCAP
```shell
$ masscan 10.0.0.1 --pcap <filename>
```

Print packets in terminal (ok in low rate but RIP terminal with high rates)
```shell
$ masscan 10.0.0.1 --packet-trace
```


### Target speficication

Target speficication
```shell script
$ masscan 10.0.0.1
$ masscan 10.0.0.0/24 192.168.1.0/24
```

Exclude IP file
```shell script
$ masscan 10.0.0.1/24 --excludeFile <file>
```


Exclude a single IP from the scan
```shell script
$ masscan 180.215.0.0/16 --exclude=180.215.122.120
```

### Port specification

Port specification
```shell script
$ masscan 10.0.0.0.1 -p 80
$ masscan 10.0.0.0.1 -p 0-65535
$ masscan 10.0.0.0.1 -p 80,443
```

UDP Scan
```shell script
$ masscan 10.0.0.0.1 -pU 53
```


### Output

Output in binary mode
```shell
$ massscan 10.1.1.1/24 -p 80 -oB <filename>
```

Output in XML format
```shell
$ massscan 10.1.1.1/24 -p 80 -oX <filename>
```

Output in grepable format
```shell
$ massscan 10.1.1.1/24 -p 80 -oG <filename>
```

Output in JSON format
```shell
$ massscan 10.1.1.1/24 -p 80 -oJ <filename>
```

Output in simple list format
```shell
$ massscan 10.1.1.1/24 -p 80 -oL <filename>
```

Read a binary output and writes it to the console
```shell
$ masscan --readscan bin-test.scan
```

Read a binary scan and convert it to another format
```shell
$ masscan --readscan bin-test.scan -oX bin-test.xml
```


Also see
-------

- [Masscan CLI](https://github.com/robertdavidgraham/masscan)
- [Kali Masscan](https://www.kali.org/tools/masscan/)