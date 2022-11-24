---
title: Tcpdump
date: 2022-11-24 14:31:01
icon: icon-yingyongzhongxin-pingtai-chanpin-07
background: bg-blue-400
tags:
    - network
categories:
    - Linux Command
intro: |
    Tcpdump is a commandline tool that is used to dump traffic on a network. This tool comes in hand when you want to analyse network captures within the command line. Basically it can do most of the wireshark job.
    **NOTE** This guide might not be complete it just serve as a reference to me.
---

## Options {.cols-1}

### Options

The following are some of options that I prefer when using `tcpdump` for my daily use.

```shell
tcpdump [OPTIONS]

-i any : Listen to all the interfaces
-i virbr0: Listen to a specific interface virbr0
-D: Show the list of available interface
-n: Don't resolve the hostnames
-nn: Don't resolve hostnames or port names.
-q: quite output
-t: Don't print a timestamp on each dump line.
-tttt: Give maximally human-readbale timestamp output
-X: Show the packet's contents in both HEX ad ASCII
-XX: Same as -X but shows the ethernet header.
-v, -vv, -vvv: Being more verbose(increase number of packet information)
-c: Only capture number of packets and stop
-s: Define the snaplength(size) of the capture in bytes. Use -s0 to get
everything.
-S: Print absolute sequence numbers.
-e: Get the ethernet header as well
-E: Decrypt IPSEC traffic by providing an encryption key.
```

### Expressions

`tcpdump` allow us to use expression so we can narrow down our solution to get
exactly what we're looking for.

There are 3 types of expression: `type`, `dir` and `proto`

+ Type options are: `host`,`net`, and `port`
+ Direction are: `src` and `dst`
+ Protocol : `tcp`,`udp`,`icmp`,`ah` etc

## Examples {.cols-1}

### Basic Examples

Basic communication to see what happens on the network

```bash
$ tcpdump -i any
```

Monitor specific interface

```bash
$ tcpdump -i virbr0
```
Raw output view with verbose output,no host/port resolution,absolute sequence number and human-readable timestamps.

```bash
$ tcpdump -ttttnnvvS
```

Find traffic by IP

```bash
$ tcpdump host 192.168.122.131
```

Seeing packets with HEX output

```bash
$ tcpdump -nnvXSs 0 -c1 icmp
```

Filtering by Source and Destination

```bash
$ tcpdump src 192.168.122.131
$ tcpdump dst 192.168.122.14
```

Finding packets by network

```bash
$ tcpdump net 192.168.122.0/24
```

Show traffic related to a specific port
```bash
$ tcpdump port 3389
```

Show traffic of one protocol
```bash
$ tcpdump icmp
```

Show only IPv6 Traffic
```bash
$ tcpdump ip6
```


Find traffic using Port ranges
```bash
$ tcpdump portrange 21-25
```

Find traffic base on packet size
```bash
$ tcpdump less 32
$ tcpdump greater 32
$ tcpdump <= 102
```

Writing captures to a file
```bash
$ tcpdump port 80 -w output
```

Reading from pcap files
```bash
$ tcpdump -r output.pcap
```

## More Examples  {.cols-2}

### Options Combination
#### Options Combination
+ AND : `and` or `&&`
+ OR : `or` or `||`
+ EXCEPT : `not` or `!`
{.marker-round}
#### ↓ Examples
```bash
$ tcpdump -nnvvS src 192.168.122.1 and dst port 4444
```

### Complex grouping
#### Complex grouping
Complex grouping and special characters For complex grouping we use `()` to specify our options
#### ↓ Examples
```bash
$ tcpdump 'src 192.168.122.84 and (dst port 4444 or 22)'
```

### URGENT packets
#### URGENT packets
Show all URGENT (URG) packets
#### ↓ Examples
```bash
$ tcpdump 'tcp[13] & 32!=0'
```

### ACKNOWLEDGE packets
#### ACKNOWLEDGE packets
Show all ACKNOWLEDGE(ACK) packets
#### ↓ Examples
```bash
$ tcpdump 'tcp[13] & 16!=0'
```

### PUSH packets
#### PUSH packets
   Show all PUSH (PSH) packets\
#### ↓ Examples
```bash
$ tcpdump 'tcp[13] & 8!=0'
```

### RST packets
#### RST packets
Show all RESET (RST) packets
#### ↓ Examples
```bash
$ tcpdump 'tcp[13] & 4!=0'
```

### SYNCHRONIZE packets
#### SYNCHRONIZE packets
Show all SYNCHRONIZE (SYN) packets
#### ↓ Examples
```bash
$ tcpdump 'tcp[13] & 2!=0'
```

### FINISH packets
#### FINISH packets
Show all FINISH (FIN) packets
#### ↓ Examples
```bash
$ tcpdump 'tcp[13] & 1!=0'
```

### SYNACK packets
#### SYNACK packets
Show all SYNCRONIZE/ACKNOWLEDGE (SYNACK) packets
#### ↓ Examples
```bash
$ tcpdump 'tcp[13]=18'
```

### tcpflags syntax
#### tcpflags syntax
Alternative we could also use `tcpflags` syntax
#### ↓ Examples
```bash
$ tcpdump 'tcp[tcpflags] == tcp-syn'
$ tcpdump 'tcp[tcpflags] == tcp-rst'
$ tcpdump 'tcp[tcpflags] == tcp-fin'
```

### malformed/malicious packets

```bash
Identifying malformed/malicious packets.

    + Packets with both rst and syn flags shouldnt be the case.
             $ tcpdump 'tcp=[13] = 6'
    + Find cleartext http get requests
             $ tcpdump 'tcp[32:4] = 0x47455420'
    + Find ssh connection on any port via (banner text)
             $ tcpdump 'tcp[(tcp[12]>>2):4] = 0x5353482D'
```