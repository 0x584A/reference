---
title: Gobuster
date: 2022-11-22 12:05:24
icon: icon-style
background: bg-amber-200
tags:
  - recon
  - port scan
  - path fuzzing
categories:
    - Reconnaissance
intro: |
      Gobuster CheatSheet - In this CheatSheet, you will find a series of practical example commands for running Gobuster and getting the most of this powerful tool.
---


## Common Gobuster Commands {.cols-2}

### dns Mode {.row-span-2}

dns Mode

```bash {.wrap}
gobuster dns -d example.com -t 50 -w common-names.txt
```

```bash {.wrap}
gobuster dns -d example.com-w ~/wordlists/subdomains.txt
```

With Show IP

```bash {.wrap}
gobuster dns -d example.com -w ~/wordlists/subdomains.txt -i
```

Base domain validation warning when the base domain fails to resolve

```bash {.wrap}
gobuster dns -d example.com -w ~/wordlists/subdomains.txt -i
```

Wildcard DNS is also detected properly:

```bash {.wrap}
gobuster dns -d 0.0.1.xip.io -w ~/wordlists/subdomains.txt
```

### dir Mode

dir Mode

```bash {.wrap}
gobuster dir -u https://example.com -w ~/wordlists/shortlist.txt
```

With content length

```bash {.wrap}
gobuster dir -u https://example.com -w ~/wordlists/shortlist.txt -l
```


### vhost Mode

vhost Mode

```bash {.wrap}
gobuster vhost -u https://example.com -w common-vhosts.txt
```

s3 Mode

```bash {.wrap}
gobuster s3 -w bucket-names.txt
```

### Available Modes

| Switch | Description                                                          |
| ------ | -------------------------------------------------------------------- |
| dir    | the classic directory brute-forcing mode                             |
| dns    | DNS subdomain brute-forcing mode                                     |
| s3     | Enumerate open S3 buckets and look for existence and bucket listings |
| vhost  | irtual host brute-forcing mode (not the same as DNS!)                |

## Mode Options {.cols-2}

### DNS Mode Options 

| Short Switch | Long Switch        | Description                                                  |
| ------------ | ------------------ | ------------------------------------------------------------ |
| -h,          | --help             | help for dns                                                 |
| -d,          | --domain string    | The target domain                                            |
| -r,          | --resolver string  | Use custom DNS server (format server.com or server.com:port) |
| -c,          | --show-cname       | Show CNAME records (cannot be used with '-i' option)         |
| -i,          | --show-ips         | Show IP addresses                                            |
|              | --timeout duration | DNS resolver timeout (default 1s)                            |

### DIR Mode Options {.row-span-3}

| Short Switch | Long Switch              | Description                                                                                                   |
| ------------ | ------------------------ |---------------------------------------------------------------------------------------------------------------|
| -h,          | --help                   | help for dir                                                                                                  |
| -f,          | --add-slash              | Append / to each request                                                                                      |
| -c,          | --cookies string         | Cookies to use for the requests                                                                               |
| -e,          | --expanded               | Expanded mode, print full URLs                                                                                |
| -x,          | --extensions string      | File extension(s) to search for                                                                               |
| -r,          | --follow-redirect        | Follow redirects                                                                                              |
| -H,          | --headers stringArray    | Specify HTTP headers, -H 'Header1: val1' -H 'Header2: val2'                                                   |
| -l,          | --include-length         | Include the length of the body in the output                                                                  |
| -k,          | --no-tls-validation      | Skip TLS certificate verification                                                                             |
| -n,          | --no-status              | Don't print status codes                                                                                      |
| -P,          | --password string        | Password for Basic Auth                                                                                       |
| -p,          | --proxy string           | Proxy to use for requests `http(s)://host:port`                                                                    |
| -s,          | --status-codes string    | Positive status codes (will be overwritten with status-codes-blacklist if set) (default "200,204,301,302,307,401,403") |
| -b,          | --status-codes-blacklist | string Negative status codes (will override status-codes if set)                                              |
|              | --timeout duration       | HTTP Timeout (default 10s)                                                                                    |
| -u,          | --url string             | The target URL                                                                                                |
| -a,          | --useragent string       | Set the User-Agent string (default "gobuster/3.1.0")                                                          |
| -U,          | --username string        | Username for Basic Auth                                                                                       |
| -d,          | --discover-backup        | Upon finding a file search for backup files                                                                   |
|              | --wildcard               | Force continued operation when wildcard found                                                                 |

### vhost Mode Options

| Short Switch | Long Switch           | Description                                                |
| ------------ | --------------------- | ---------------------------------------------------------- |
| -h           | --help                | help for vhost                                             |
| -c           | --cookies string      | Cookies to use for the requests                            |
| -r           | --follow-redirect     | Follow redirects                                           |
| -H           | --headers stringArray | Specify HTTP headers, -H 'Header1: val1' -H 'Header2: val2' |
| -k           | --no-tls-validation   | Skip TLS certificate verification                          |
| -P           | --password string     | Password for Basic Auth                                    |
| -p           | --proxy string        | Proxy to use for requests `http(s)://host:port`             |
|              | --timeout duration    | HTTP Timeout (default 10s)                                 |
| -u           | --url string          | The target URL                                             |
| -a           | --useragent string    | Set the User-Agent string (default "gobuster/3.1.0")       |
| -U           | --username string     | Username for Basic Auth                                    |

### Global Flags

| Short Switch | Long Switch       | Description                                          |
| ------------ | ----------------- | ---------------------------------------------------- |
| -z           | --no-progress     | Don't display progress                               |
| -o           | --output string   | Output file to write results to (defaults to stdout) |
| -q           | --quiet           | Don't print the banner and other noise               |
| -t           | --threads int     | Number of concurrent threads (default 10)            |
| -i           | --show-ips        | Show IP addresses                                    |
|              | --delay duration  | DNS resolver timeout (default 1s)                    |
| -v,          | --verbose         | Verbose output (errors)                              |
| -w           | --wordlist string | Path to the wordlist                                 |



Also see {.cols-1}
--------
- [feroxbuster](https://github.com/epi052/feroxbuster) _(https://github.com/epi052/feroxbuster)_
- [fuzz](https://github.com/ffuf/ffuf) _(https://github.com/ffuf/ffuf)_
- [wfuzz](http://www.edge-security.com/wfuzz.php) _(http://www.edge-security.com/wfuzz.php)_
- [dirsearch](https://github.com/maurosoria/dirsearch) _(https://github.com/maurosoria/dirsearch)_