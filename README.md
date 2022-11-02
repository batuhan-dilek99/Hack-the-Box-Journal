# Photobomb
## Part 1: Discovery
### Scanning
I have scanned the ip address of the machine using nmap
``` sudo nmap -sVC -T4 -p- machine_ip ```<br/>
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
### Investigating the website
When the website is visited, It is not available. We need to configure our local dns server to make possible to visit the website. <br/>
``` sudo vi /etc/hosts```<br/>
After opening the vi editor, insert: ```machine_ip photobomb.htb```. <br/>
Now the website is visitable.<br/>
It is possible to find ```photobomb.js``` when the source code is inspected.<br/>
go to this URL : ```http://photobomb.htb/photobomb.js``` <br/>
we can see that with this url : ```http://pH0t0:b0Mb!@photobomb.htb/printer``` we can log into the site with credentials. <br/>

### Burpsuite
In this website we can download a bunch of images. We can select resolution and the filetype.<br/>
When we analyze the request made when the download button has clicked, it is visible that image name, filetype and resolution are being sent to the backend. <br/>
```photo=wolfgang-hasselmann-RLEgmd1O7gs-unsplash.jpg&filetype=jpg&dimensions=3000x2000``` <br/>
We can use this opening to feed our reverse shell into the sever.<br/>


 ## Part 2 : Exploiting
 ### Preping a reverse shell
 I wanted to make this website pull a shell script from my ip. For this purpose; <br/>
 I have opened a netcast listener on port 443. When the shell script has run, listener will catch and makes us traverse the server directories.<br/>
 We are going to make the website pull shell script from a python http server which is running on port 8080.<br/>
 
