# Grayhatcon CTF

## Description
This write up will show how to solve the CTF for "Grayhatcon CTF"

**Difficulty:** Moderate

Hint 1: The registration form might have fields you missed!

## Solution 1st flag
Task opens when going to page: Someone's listed all the usernames and passwords for HackerOne on HackerBay! Hack your way in and delete the listing before someone buys them!</br>

According to the hint, there might be missing fields in the registration form.</br>
In the GET request of the Registration I don't see any interesting files.</br>
When creating a new username, the POST request has 2 cookies: a token and a userhash.</br>
