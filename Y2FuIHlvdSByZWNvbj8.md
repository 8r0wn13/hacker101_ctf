# Y2FuIHlvdSByZWNvbj8/

## Description
This write up will show how to solve the CTF for "Y2FuIHlvdSByZWNvbj8/"

**Difficulty:** Moderate

## Solution 1st flag
Fuzzing for files and directories, I get the following result:</br>
```
.htaccess
.hta
.htpasswd
admin
server-status
```
All these end-points return a `403 Forbidden` error.</br>

