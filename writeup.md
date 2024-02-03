## I Encourage Everyone To Try Completing The Tasks Without Any Help. Only Use This Sheet If You're Stuck.

## Table of content
- [Task 1  Introduction](#task-1---Introduction)
- [Task 2  Find all the flags](#task-2---Find-all-the-flags)

# Task 1 - Introduction:
Deploy the machine and connect to our network. 
```
No answer neeeded
```

# Task 2  - Find all the flags:

### Port Scan
It identified ports 21,22,80,111,2049 and 3306 as open. Running a more detailed scan on found open ports with `nmap` reveals their respective versions.
```
nmap -p- -sS -sV -sC -A -O -T5 <machine-ip> -v 
```

```
PORT     STATE SERVICE VERSION
21/tcp   open  ftp     vsftpd 3.0.5
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
|_Can't get directory listing: TIMEOUT
| ftp-syst: 
|   STAT: 
| FTP server status:
|      Connected to ::ffff:10.0.2.40
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      At session startup, client count was 3
|      vsFTPd 3.0.5 - secure, fast, stable
|_End of status
22/tcp   open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.11 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 34:40:3e:2b:7a:35:be:1c:44:80:95:1f:2b:b9:ae:8e (RSA)
|   256 1c:48:d7:f4:73:42:3e:7d:6c:5c:be:70:18:bb:21:8e (ECDSA)
|_  256 ed:c4:fc:21:c0:65:17:60:1a:e7:ad:f5:0b:5e:1a:41 (ED25519)
80/tcp   open  http    nginx 1.18.0 (Ubuntu)
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-server-header: nginx/1.18.0 (Ubuntu)
|_http-title: Did not follow redirect to http://egypt.thm/
111/tcp  open  rpcbind 2-4 (RPC #100000)
| rpcinfo: 
|   program version    port/proto  service
|   100000  2,3,4        111/tcp   rpcbind
|   100000  2,3,4        111/udp   rpcbind
|   100000  3,4          111/tcp6  rpcbind
|   100000  3,4          111/udp6  rpcbind
|   100003  3           2049/udp   nfs
|   100003  3           2049/udp6  nfs
|   100003  3,4         2049/tcp   nfs
|   100003  3,4         2049/tcp6  nfs
|   100005  1,2,3      32866/udp6  mountd
|   100005  1,2,3      51103/tcp   mountd
|   100005  1,2,3      55829/udp   mountd
|   100005  1,2,3      57883/tcp6  mountd
|   100021  1,3,4      38361/tcp   nlockmgr
|   100021  1,3,4      46649/tcp6  nlockmgr
|   100021  1,3,4      51226/udp   nlockmgr
|   100021  1,3,4      57375/udp6  nlockmgr
|   100227  3           2049/tcp   nfs_acl
|   100227  3           2049/tcp6  nfs_acl
|   100227  3           2049/udp   nfs_acl
|_  100227  3           2049/udp6  nfs_acl
2049/tcp open  nfs     3-4 (RPC #100003)
3306/tcp open  mysql   MySQL 5.5.5-10.3.39-MariaDB-0ubuntu0.20.04.2
| mysql-info: 
|   Protocol: 10
|   Version: 5.5.5-10.3.39-MariaDB-0ubuntu0.20.04.2
|   Thread ID: 47
|   Capabilities flags: 63486
|   Some Capabilities: Support41Auth, Speaks41ProtocolOld, SupportsLoadDataLocal, FoundRows, ODBCClient, IgnoreSpaceBeforeParenthesis, Speaks41ProtocolNew, InteractiveClient, SupportsTransactions, IgnoreSigpipes, ConnectWithDatabase, DontAllowDatabaseTableColumn, LongColumnFlag, SupportsCompression, SupportsAuthPlugins, SupportsMultipleResults, SupportsMultipleStatments
|   Status: Autocommit
|   Salt: L&wHAL9xsZF1auKR>|w`
|_  Auth Plugin Name: mysql_native_password
```
### Web Application Enumeration
Going to the webpage using the given IP address redirects to egypt.thm domain, but fails to open a website.
```
80/tcp   open  http    nginx 1.18.0 (Ubuntu)
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-server-header: nginx/1.18.0 (Ubuntu)
|_http-title: Did not follow redirect to http://egypt.thm/
```
