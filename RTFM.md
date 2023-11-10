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
status - 200 OK (duplicate of the previous one)
user - 400 Bad Request (`"error":"X-Token header authentication missing"`)
user - 400 Bad Request (duplicate of the previous one)
secrets - 403 Forbidden (`"error":"Access not allowed from your IP address"`)

## Solution 3th flag
The user endpoint returns that it needs an X-Token.</br>
Just adding the X-Token returns `Invalid Token`</br>
When changing the GET request to a POST request and sending the request without a token returns `"error":"Missing username and password field"`.</br>
This indidcates, it needs a username and password.</br>
Include the parameters username and password with random values and send the request.</br>
This will reveal the flag.</br>

## Solution 4th flag
In the previous flag, it returned it is possible to log at `/api/v1/user/login`</br>
In Burp, when we add `/login` to the request and send it again, it returns a token.</br>

Now use this token as the value for X-Token, hence change the following:</br>
```
PUT /api/v1/user HTTP/2
X-Token: ae2e1659352ceb5b6d2b658d918e8a46
```

When sending the request, it returns an error: `"error":"No updatable fields supplied"`
To figure out what updatable field is available, we can brute force the field.</br>
In the request add `%s=<<give your username you created>>`</br>

%s as I am using Turbo Intruder in Burp, that's the value we're fuzzing for. In ffuf you would replace with FUZZ.</br>
I used the common.txt word list.</br>
In Turbo Intruder, I filter all the request which have `No updatable fields supplied` in the request response.</br>
There is 1 updatable field: `avatar`.</br>

In Burp Repeater, when sending a request with `avatar=<<give your username you created>>` it returns another error: `"error":"Avatar resource should start with http:\/\/ or https:\/\/ "`</br>
This means avatar's value needs to be a link.</br>
Let's try `avatar=http://localhost/api/v1/secrets`

## Solution 5th flag
Back to fuzzing the api endpoints for parameters, there is a parameter verbose:</br>
`ffuf -w ~/Documents/bug_bounty/lists/parameters/burp-parameter-names.txt -u https://86231f4f90fa0093ae16f65a82952418.ctf.hacker101.com/api/v1/status?FUZZ=<<give your username you created>> -fs 1-20 -s`

Going to this endpoint `https://86231f4f90fa0093ae16f65a82952418.ctf.hacker101.com/api/v1/status?verbose=<<give your username you created>>` will give the flag.</br>

## Solution 6th flag
In the response from the seems to be another api end-point: `/api/v2/admin/user-list`.</br>
When going to that end-point, it returns an error `"error":"X-Session header authentication missing"`.</br>
When adding a the X-Token and use it for X-Session, it returns the flag.</br>

## Solution 7th flag




## Solution 8th flag






username=zcvzcv&password=zvzv
