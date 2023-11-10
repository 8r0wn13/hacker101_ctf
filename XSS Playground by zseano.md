# XSS Playground by zseano

## Description
This write up will show how to solve the CTF for "XSS Playground by zseano"

**Difficulty:** Moderate

Hints are not being provided for multiple reasons:</br>
1. It is better to figure it out yourself and I don't want to give spoilers, especially if you don't need one
2. It is only possible to get a hint every once in a while, i.e. it is not possible to get all the hints one after another. This will take too much time to "wait" for all the hints

Feeling up to a challenge?

Can you find all of the XSS on this page? Use a keen eye and see if you can discover the following types of XSS:

Pop-up of available XSS in this page:</br>

5 Reflective Cross Site Scripting</br>
3 Stored Cross Site Scripting</br>
2 DOM-Based Cross Site Scripting</br>
1 CSP-Bypass Cross Site Scripting</br>
1 use of XSS to leak "something"</br>
Good luck! - zseano & HackerOne</br>

## Solution 1st flag
When looking at custom.js, there is a function to get the email: `/api/action.php?act=getemail`</br>
This requires an additional security header: `X-SAFEPROTECTION:enNlYW5vb2Zjb3Vyc2U=`</br>
In burp it didn't work, as the security header needs to be capitalized and Burp is making it title case.</br>
In curl it didn't work either, as on the background apparently something similar happened as in Burp.</br>
After some research on the internet, this can be avoided, by setting the http header to HTTP1.1, while the standard request is HTTP2.</br>
This didn't work in Burp either, but when running the following command in the terminal, it worked:</br>
`curl https://e884d55c39556e615b0eb31104e29e5d.ctf.hacker101.com/api/action.php?act=getemail -H X-SAFEPROTECTION:enNlYW5vb2Zjb3Vyc2U= --http1.1`
This will return the flag, however, the flag is missing `FLAG$` at the end, hence don't forget to add that. :-)
