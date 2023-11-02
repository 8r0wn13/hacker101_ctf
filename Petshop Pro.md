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
Brute-force first the username.</br>

I tried to brute-force it with OWASP ZAP and Hydra, but OWASP ZAP doesn't support filtering for strings in reponse bodies and Hydra was incredibly slow.</br>
Burp Community doesn't support Intruder, unless it is very slow, however, after searching on Google it turns out there is Turbo Intruder, which is supported by Burp Community as well.</br>
With this I found 1 username, which we can now bruteforce with passwords, also with Turbo Intruder in Burp.</br>

After finding the password for this specific user and logging into the application, it will show the flag right away.</br>

## Solution 3rd flag
When logged in, it is possible to edit the details of the pictures.</br>
Try with the following script `<img src=x onerror=alert(1)>` to save it in one of the fields.</br>
Once the payload is successfully saved, add the item to your cart and check your cart with items (do not go to Check Out).</br>
When checking the items in the cart, it will show the flag.</br>
