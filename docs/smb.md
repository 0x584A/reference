---
title: SMB
date: 2022-11-24 15:17:17
icon: icon-style
background: bg-[#d33731]
tags:
    - smb
    - smbclient
categories:
    - Attack Service
intro: |
    Quick attack smbclient cheat sheet.
---

## notes {.cols-1}

```powershell
List of SMB versions and corresponding Windows versions:
    CIFS – Windows NT 4.0
    SMB1 – Windows 2000, XP and Windows 2003.
    SMB2 – Windows Vista SP1 and Windows 2008 (supported in Samba 3.6)
    SMB2.1 – Windows 7 and Windows 2008 R2 (Samba 4.0)
    SMB3 – Windows 8 and Windows 2012 (Samba 4.2)
    SMB3.02 – Windows Server 2012 R2 and Windows 8.1 (not supported in Samba)
    SMB3.1.1 – Windows Server 2016 and Windows 10 (not supported in Samba)

> https://learn.microsoft.com/en-us/openspecs/windows_protocols/ms-smb2/a64e55aa-1152-48e4-8206-edd96444e7f7
```

## smbclient {.cols-2}

### Basic usage {.row-span-4}
**List shares on a machine**
#### ↓ Example
```yaml {.wrap}
$ smbclient -L <<target>>
```

**List shares on a machine using a valid username + password**
#### ↓ Example
```yaml {.wrap}
$ smbclient -L <<target>> -U username%password
```

**Connect to a valid share with username + password**
#### ↓ Example
```yaml {.wrap}
$ smbclient //<target>/<share$> -U username%password
```

**List files on a specific share**
#### ↓ Example
```yaml {.wrap}
$ smbclient //<target>/<share$> -c 'ls' password -U username
```

**List files on a specific share folder inside the share**
#### ↓ Example
```yaml {.wrap}
$ smbclient //<target>/<share$> -c 'cd folder; ls' password -U username
```


### download
**Download a file from a specific share folder**
```yaml {.wrap}
$ smbclient //<target>/<share$> -c 'cd folder;get desired_file_name' password -U username
```

### Copy

**Copy a file to a specific share folder**
```yaml {.wrap}
$ smbclient //<target>/<share$> -c 'put /tmp/my_local_file.txt .\target_folder\target_file.txt' password -U username
```

### Create folder

**Create a folder in a specific share folder**
```yaml {.wrap}
$ smbclient //<target>/<share$> -c 'mkdir .\target_folder\new_folder' password -U username
```

### Rename file

**Rename a file in a specific share folder**
```yaml {.wrap}
$ smbclient //<target>/<share$> -c 'rename current_file.txt new_file.txt' password -U username
```



### recursive download
**Download all recursive download**
```yaml {.wrap}
smbclient //<IP>/<share>
> mask ""
> recurse
> prompt
> mget *
```

### protocol version
**Downgrade the protocol version**
```yaml {.wrap}
$ smbclient //<IP>/'Bob Share' --option='client min protocol=nt1'
```

## enum4linux {.cols-2}

### anonymous session
```yaml {.wrap}
$ enum4linux -a <target>
```

### authenticated session
```yaml {.wrap}
$ enum4linux -a <target> -u <user> -p <pass>
```

### Users enumeration
```yaml {.wrap}
$ enum4linux -u <user> -p <pass> -U <target>
```

### Group and members enumeration
```yaml {.wrap}
$ enum4linux -u <user> -p <pass> -G <target>
```

### Password policy
```yaml {.wrap}
$ enum4linux -u <user> -p <pass> -P <target>
```

## Nmap Script {.cols-1}

### Enum Users
```yaml {.wrap}
$ nmap -p 445 --script smb-enum-users <target> --script-args smbuser=username,smbpass=password,smbdomain=domain

$ ...snip... --script-args smbuser=username,smbhash=LM:NTLM,smbdomain=domain

$ ...snip... --script-args smbusername=User1,smbpass=Pass@1234,smbdomain=workstation 

$ ...snip... --script-args smbusername=User1,smbhash=LM:NTLM,smbdomain=mydomain 
```

### Enum Groups
```yaml {.wrap}
$ nmap -p 445 --script smb-enum-groups <target> --script-args smbuser=username,smbpass=password,smbdomain=domain

$ ...snip... --script-args smbuser=username,smbhash=LM:NTLM,smbdomain=domain
```

### Enum Shares
```yaml {.wrap}
$ nmap -p 445 --script smb-enum-shares <target> --script-args smbuser=username,smbpass=password,smbdomain=domain

$ ...snip... --script-args smbuser=username,smbpass=LM:NTLM,smbdomain=domain
```

### OS Discovery
```yaml {.wrap}
$ nmap -p 445 --script smb-os-discovery <target>
```

### SMB Vulnerabilities on Windows
```yaml {.wrap}
$ nmap -p139,445 --script smb-vuln* <target>

$ nmap -p 445 --script smb-vuln-ms06-025 <target>
 
$ nmap -p 445 --script smb-vuln-ms07-029 <target>

$ nmap -p 445 --script smb-vuln-ms08-067 <target>

$ nmap -p 445 --script smb-vuln-ms10-054 <target>

$ nmap -p 445 --script smb-vuln-ms10-061 <target>

$ nmap -p 445 --script smb-vuln-ms17-010 <target>

$ nmap -p 445 --script smb-vuln-cve-2017-7494 <target>
```

## OSCP Tips {.cols-2}

### Tips

**View the data package to get version information**
```powershell {.wrap}
$ tcpdump -i tun0 -p -s 0 -w smb.pcap
```

Also see
--------

- [Patator](https://github.com/lanjelot/patator) _(https://github.com/lanjelot/patator)_
- [hydra](https://github.com/vanhauser-thc/thc-hydra) _(https://github.com/vanhauser-thc/thc-hydra)_


