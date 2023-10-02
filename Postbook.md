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







This make a request and will show the flag:</br>
`^FLAG^b2847b8fedbcde8db7c2c99f605364a6dbaa0f24a4cb02587e45707ce6f80f60$FLAG$`

## Solution 3rd flag





## Solution 4th flag





## Solution 5th flag





## Solution 6th flag





## Solution 7th flag



