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
The 400 Bad Request response found `api/v1/user`, which we might come back to later, because it gives an error: `"error":"X-Token header authentication missing"`</br>

## Solution 2nd flag
For the first flag, we used complete endpoints.</br>
Fuzz again, but now not on the root of the web application, but `/api/v1/` with api-endpoints.res.txt.</br>
This will result in a 4 200 OK responses, 2 400 Bad Request responses and 1 403 Forbidden response.</br>
status - 200 OK
config - 200 OK (contains flag)
config - 200 OK (duplicate of the previous one)
status - 200 OK
user - 400 Bad Request (`"error":"X-Token header authentication missing"`)
user - 400 Bad Request (duplicate of the previous one)
secrets - 403 Forbidden (`"error":"Access not allowed from your IP address"`)

## Solution 3rd flag
Secrets looks interesting.</br>
I've been fuzzing on `/api/v1/secrets` and added `X-Forwarded-For: %s`</br>
Using 10 ranges and 192 ranges.</br>
I also tried manually 127.0.0.1, but that didn't work either.</br>
With a script we can generate all ip addresses and run it during the night.</br>



## Solution 4th flag





## Solution 5th flag






## Solution 6th flag





## Solution 7th flag




## Solution 8th flag
