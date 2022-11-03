
<h1 align=center>Shoppy</h1>

## Part 1 : Discovery
## Scanning
```sudo nmap -sVC -T4 -p- target_ip``` gives us the result : 
```
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.4p1 Debian 5+deb11u1 (protocol 2.0)
| ssh-hostkey: 
|   3072 9e:5e:83:51:d9:9f:89:ea:47:1a:12:eb:81:f9:22:c0 (RSA)
|   256 58:57:ee:eb:06:50:03:7c:84:63:d7:a3:41:5b:1a:d5 (ECDSA)
|_  256 3e:9d:0a:42:90:44:38:60:b3:b6:2c:e9:bd:9a:67:54 (ED25519)
80/tcp open  http    nginx 1.23.1
|_http-title: Did not follow redirect to http://shoppy.htb
|_http-server-header: nginx/1.23.1
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```
This box is hosting a web application on port 80 called shoppy.htb. <br/>

This website is unreachable. We have to specifiy this machines' IP and domain into our local DNS Resolver.<br/>
- execute : ```sudo vi /etc/hosts/```
- insert : ``` target_ip  shoppy.htb```
- save and quit

## Investigating the website
We should discover if there are any subdomains or virtual hosts that are serving under the same ip

### Finding directories 
- execute ```gobuster dir -u http://shoppy.htb -w /usr/share/wordlists/dirb/big.txt``` to find directories under this domain<br/> result :
```
gobuster dir -u http://shoppy.htb -w /usr/share/wordlists/dirb/big.txt
===============================================================
Gobuster v3.1.0
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://shoppy.htb
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                /usr/share/wordlists/dirb/big.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.1.0
[+] Timeout:                 10s
===============================================================
2022/11/03 16:30:35 Starting gobuster in directory enumeration mode
===============================================================
/ADMIN                (Status: 302) [Size: 28] [--> /login]
/Admin                (Status: 302) [Size: 28] [--> /login]
/Login                (Status: 200) [Size: 1074]           
/admin                (Status: 302) [Size: 28] [--> /login]
/assets               (Status: 301) [Size: 179] [--> /assets/]
/css                  (Status: 301) [Size: 173] [--> /css/]   
/exports              (Status: 301) [Size: 181] [--> /exports/]
/favicon.ico          (Status: 200) [Size: 213054]             
/fonts                (Status: 301) [Size: 177] [--> /fonts/]  
/images               (Status: 301) [Size: 179] [--> /images/] 
/js                   (Status: 301) [Size: 171] [--> /js/]     
/login                (Status: 200) [Size: 1074]               
                                                               
===============================================================
2022/11/03 16:33:12 Finished
===============================================================

```
<br/>
We can see that there are some login and admin directories that opens a new page and asks user to login. We may bypass authentication by SQLI.
- go to http://shoppy.htb/login
- enter ```admin'||'1=1``` into username
This will bring us the admin page that we can search anything in the admin page. <br/>
- I have decided to use ```admin'||'1=1``` once again here to see if there are anything hidden in the Database.
- This gives us a download button which will download some creds about the users.
