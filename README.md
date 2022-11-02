# Photobomb
## Part 1 : Discovery
### Scanning
I have scanned the ip address of the machine using nmap
``` sudo nmap -sVC -T4 -p- machine_ip ```
According to the nmap result there are 2 services serving on port 22 and port 80. <br/>
```
PORT   STATE SERVICE REASON         VERSION
22/tcp open  ssh     syn-ack ttl 63 OpenSSH 8.2p1 Ubuntu 4ubuntu0.5 (Ubuntu Linux; protocol 2.0)
80/tcp open  http    syn-ack ttl 63 nginx 1.18.0 (Ubuntu)
|_http-server-header: nginx/1.18.0 (Ubuntu)
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-title: Did not follow redirect to http://photobomb.htb/
```
