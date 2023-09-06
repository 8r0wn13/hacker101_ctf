# Micro-CMS v2

## Description
This write up will show how to solve the CTF for "Micro-CMS v2"

**Difficulty:** Moderate

Hints are not being provided for multiple reasons:</br>
1. It is better to figure it out yourself and I don't want to give spoilers, especially if you don't need one
2. It is only possible to get a hint every once in a while, i.e. it is not possible to get all the hints one after another. This will take too much time to "wait" for all the hints

## Solution 1st flag
When logging in, in the username give the following:</br>
`'UNION SELECT '123' AS password FROM admins WHERE '1' = '1` and for the password `123`</br>

With the above statement in the username and password `123`, we're able to log in</br>
There is a Private Page, once clicked it will show the flag:

![alt](https://github.com/8r0wn13/hacker101_ctf/blob/main/images/Screenshot%20from%202023-09-06%2021-59-38.png?raw=true)

## Solution 2nd flag
Click on Markdown Test and Edit Page</br>
In the terminal execute the below command:</br>
`curl -v -X POST https://038e18240aba44f163b46d8f251f1764.ctf.hacker101.com/page/edit/2`



## Solution 3rd flag
When trying to edit or create a new page it requires login details</br>
Try to login with just the username</br>
Most probably it will return `Unknown User`</br>
Open ZAP and give a username and password</br>
Right click the POST request > attack > Fuzz</br>
Double click the username > Add > Add > File > Select a file with usernames to try > Add > OK > Start Fuzzer</br>
Normally the Size Resp. Body is 451 bytes, when an existing user is found it will have a different number of bytes (455): `nicolette`</br>

Change the username to `nicolette` do the same exercise for the password</br>
Right click the POST request > attack > Fuzz</br>
Change the username to `nicolette`
Double click the password > Add > Add > File > Select a file with passwords to try > Add > OK > Start Fuzzer</br>
Normally the Size Resp. Body is 455 bytes, when an existing user is found it will have a different number of bytes (76): `wilfredo`</br>
The response will show the flag:

![alt](https://github.com/8r0wn13/hacker101_ctf/blob/main/images/Screenshot%20from%202023-09-06%2021-37-55.png?raw=true)
