# Hackyholidays CTF

## Description
This write up will show how to solve the CTF for "Hackyholidays CTF"

**Difficulty:** Moderate

## Solution 1st flag
Fuzzing for files and directories, I get the following result:</br>
```
assets
favicon.ico
forum
robots.txt
```
When going to `https://c7e947b150b633c436c698e00271600a.ctf.hacker101.com/robots.txt`, it will show the first flag.</br>

## Solution 2nd flag
When go to robots.txt, besides the flag, it shows another hidden directory: `/s3cr3t-ar3a`</br>
Going to `https://c7e947b150b633c436c698e00271600a.ctf.hacker101.com/s3cr3t-ar3a`, it will show the page has moved.</br>
In Burp it doesn't show, but in Elements inspector, it shows the flag.</br>

## Solution 3rd flag
In `/s3cr3t-ar3a`, it also shows a next-page `apps-home`</br>
In `/apps-home`, it shows the boxes of all the different challenges:</br>
```
3 /people-rater: The grinch likes to keep lists of all the people he hates. This year he's gone digital but there might be a record that doesn't belong!
4 /swag-shop: Get your Grinch Merch! Try and find a way to pull the Grinch's personal details from the online shop.
5 /secure-login: Try and find a way past the login page to get to the secret area.
6 /my-diary: Hackers! It looks like the Grinch has released his Diary on Grinch Networks. We know he has an upcoming event but he hasn't posted it on his calendar. Can you hack his diary and find out what it is?
7 /hate-mail-generator: Sending letters is so slow! Now the grinch sends his hate mail by email campaigns! Try and find the hidden flag!
8 /forum: The Grinch thought it might be a good idea to start a forum but nobody really wants to chat to him. He keeps his best posts in the Admin section but you'll need a valid login to access that!
9 /evil-quiz: Just how evil are you? Take the quiz and see! Just don't go poking around the admin area!
10 /signup-manager: You've made it this far! The grinch is recruiting for his army to ruin the holidays but they're very picky on who they let in!
```
