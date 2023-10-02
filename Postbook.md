# Postbook

## Description
This write up will show how to solve the CTF for "Postbook"

**Difficulty:** Easy

Hints are not being provided for multiple reasons:</br>
1. It is better to figure it out yourself and I don't want to give spoilers, especially if you don't need one
2. It is only possible to get a hint every once in a while, i.e. it is not possible to get all the hints one after another. This will take too much time to "wait" for all the hints

## Solution 1st flag
After creating a user account and log in, click on any post</br>
In the URL it shows that there is an id</br>
In ZAP, Fuzz this id with a number range</br>
It turns out that page with id number 2 contains the flag:</br>

![alt](https://github.com/8r0wn13/hacker101_ctf/blob/main/images/Screenshot%20from%202023-10-02%2002-28-05.png?raw=true)

## Solution 2nd flag
When logged in and requests are being made an id cookie is sent</br>
I copied the cookie in Cyberchef.io and with Magic it suggests it might be Base64, Hex or Hexdump</br>
Decoding with either the aforementioned hashing, it didn't work</br>
MD5 is also often used for hashing. When decoding it online, it indeed turned out to be MD5 and it returned a number</br>
This means, the cookie is probably the id of the user logged in</br>
In order to find the right hash for the admin, we can hash each number and see which one is for the admin user</br>

![alt](https://github.com/8r0wn13/hacker101_ctf/blob/main/images/Screenshot%20from%202023-10-02%2003-01-18.png?raw=true)

In ZAP send the request for account.php (Settings) to Open/Resend with Request Editor and send a request for number 1</br>
This is right away the cookie for the admin, it shows admin as user and it shows the password for the admin</br>

![alt](https://github.com/8r0wn13/hacker101_ctf/blob/main/images/Screenshot%20from%202023-10-02%2003-02-20.png?raw=true)

Now logout as the normal user and log in with the credentials with the admin</br>

![alt](https://github.com/8r0wn13/hacker101_ctf/blob/main/images/Screenshot%20from%202023-10-02%2003-03-20.png?raw=true)

## Solution 3rd flag
There are 2 users, where we can perform a bruteforce attack on</br>
For the admin, there are no matching password in my list</br>
The user however, has the password `password`</br>

Log in with the user `user` and password `password` in order to get the flag</br>

![alt](https://github.com/8r0wn13/hacker101_ctf/blob/main/images/Screenshot%20from%202023-10-02%2023-26-53.png?raw=true)

## Solution 4th flag
When a post is created, it is possible to edit the post</br>
In the URL is the id included of that respective post</br>
When changing the id to 2, and click on `Save`, it will show the flag</br>

![alt](https://github.com/8r0wn13/hacker101_ctf/blob/main/images/Screenshot%20from%202023-10-02%2023-33-30.png?raw=true)

## Solution 5th flag





## Solution 6th flag





## Solution 7th flag



