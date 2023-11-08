# Grayhatcon CTF

## Description
This write up will show how to solve the CTF for "Grayhatcon CTF"

**Difficulty:** Moderate

Hint 1: The registration form might have fields you missed!

## Solution 1st flag
Task opens when going to page: Someone's listed all the usernames and passwords for HackerOne on HackerBay! Hack your way in and delete the listing before someone buys them!</br>

When opening the web applications and go through the source code, I don't immediately see something of interest.</br>
As part of recon, I start to fuzz for files:</br>
`ffuf -u https://d89187ac2f21751866dea3520cb35f2f.ctf.hacker101.com/s3cr3t-4dm1n/FUZZ -w ~/Documents/bug_bounty/lists/files/common.txt`</br>

With this I found the following files:</br>
```
.htpasswd               [Status: 403, Size: 315, Words: 20, Lines: 10]
.htaccess               [Status: 403, Size: 315, Words: 20, Lines: 10]
.hta                    [Status: 403, Size: 315, Words: 20, Lines: 10]
assets                  [Status: 301, Size: 389, Words: 20, Lines: 10]
dashboard               [Status: 302, Size: 0, Words: 1, Lines: 1]
favicon.ico             [Status: 200, Size: 5426, Words: 10, Lines: 4]
login                   [Status: 200, Size: 1726, Words: 430, Lines: 36]
logout                  [Status: 302, Size: 0, Words: 1, Lines: 1]
register                [Status: 200, Size: 1665, Words: 357, Lines: 33]
robots.txt              [Status: 200, Size: 38, Words: 3, Lines: 3]
robots                  [Status: 200, Size: 38, Words: 3, Lines: 3]
server-status           [Status: 403, Size: 315, Words: 20, Lines: 10]
```

When visiting `https://d89187ac2f21751866dea3520cb35f2f.ctf.hacker101.com/robots.txt` I get the following:</br>
```
User-agent: *
Disallow: s3cr3t-4dm1n/
```
That means there should be a hidden folder.</br>
When visiting `https://d89187ac2f21751866dea3520cb35f2f.ctf.hacker101.com/s3cr3t-4dm1n` it returns a 403 Forbidden error.</br>

Now fuzz for files in `s3cr3t-4dm1n` with: `ffuf -u https://d89187ac2f21751866dea3520cb35f2f.ctf.hacker101.com/s3cr3t-4dm1n/FUZZ -w ~/Documents/bug_bounty/lists/files/common.txt`</br>
In this folder there is also an .htaccess which is a configuration file for an Apache webserver.</br>
Read the data in the .htaccess file in the s3cr3t-4dm1n folder: `curl https://d89187ac2f21751866dea3520cb35f2f.ctf.hacker101.com/s3cr3t-4dm1n/.htaccess`:</br>
```
Order Deny,Allow
Deny from all
Allow from 8.8.8.8
Allow from 8.8.4.4
```
This means the s3cr3t-4dm1n folder can only be accessed from these IP's. Make the same request, but include in the X-Forwarded-For in the request:</br>
`curl https://d89187ac2f21751866dea3520cb35f2f.ctf.hacker101.com/s3cr3t-4dm1n/ -H "X-Forwarded-For: 8.8.8.8"`

This should give the flag, but it doesn't. It still gives a response 403 Forbidden.</br>

## Solution 2nd flag








