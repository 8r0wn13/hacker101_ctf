# Petshop Pro

## Description
This write up will show how to solve the CTF for "Petshop Pro"

**Difficulty:** Easy

Hints are not being provided for multiple reasons:</br>
1. It is better to figure it out yourself and I don't want to give spoilers, especially if you don't need one
2. It is only possible to get a hint every once in a while, i.e. it is not possible to get all the hints one after another. This will take too much time to "wait" for all the hints

## Solution 1st flag
Click on the various links in the application and analyze the code.</br>
The /cart page has a hidden field `type="hidden"`.</br>
Remove the hidden field tag and it should show an input box with the items in the cart.</br>

![alt](https://github.com/8r0wn13/hacker101_ctf/blob/main/images/3v44f0t6gr59oxhnro83.png?raw=true)

After changing the amounts to zero and click on Check Out, it will show the summary and the flag.</br>

## Solution 2nd flag
When checking if there are other endpoints, it seems there is a login portal accepting credentials in clear text.</br>
Trying different usernames, result in the same error message "Invalid username".</br>
Brute-force first the username with Hydra (I have Burp Community Edition and Intruder is limited in this version and therefore very slow). Hence, I use Hydra.</br>

`hydra -L names.txt -p aaa 34.209.233.57 http-post-form "/67acabbcfcd64a73d8e5f43d5cd8947d.ctf.hacker101.com/login:username=^USER^&password=^PASS^:Incorrect password" -T 32`




## Solution 3rd flag
