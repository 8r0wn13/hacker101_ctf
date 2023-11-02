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
