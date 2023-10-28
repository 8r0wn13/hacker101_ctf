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



## Solution 3rd flag
