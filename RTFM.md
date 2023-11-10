# RTFM

## Description
This write up will show how to solve the CTF for "RTFM"

**Difficulty:** Moderate

## Solution 1st flag
Hint: Wordlists will help you find something to do

The main paige states `API base located at /api/v1/`</br>
Fuzz for API endpoints based on the root page</br>
Select the root request and send it to Turbo Intruder and fuzz with api-endpoints.txt</br>
Most of it will be 404 Not Found responses, but there is one 200 OK response and one 400 Bad Request reponse.</br>
The 200 OK response found `api/v2/swagger.json` containing the flag.</br>

## Solution 2nd flag





## Solution 3rd flag




## Solution 4th flag





## Solution 5th flag






## Solution 6th flag





## Solution 7th flag




## Solution 8th flag
