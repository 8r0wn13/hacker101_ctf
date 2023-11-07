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
There are 2 solutions on the internet.</br>

The first one is to copy the getemail Javascript function to the console and add `&& console.log(t.responseText)` to onreadystatechange:</br>
```
var t=new XMLHttpRequest;
t.open("GET","/api/action.php?act=getemail",!0)
t.setRequestHeader("X-SAFEPROTECTION","enNlYW5vb2Zjb3Vyc2U=")
t.onreadystatechange = function(){ 
    this.readyState === XMLHttpRequest.DONE && this.status && console.log(t.responseText) 
}
t.send()
```

This solution doesn't work for me as it returns "Undefined". When loading the page, I get an error in the custom.js on row 71:</br>
```
1 == who.includes('zsh1') &&
document.write('<img src=/1x1.gif?' + decodeURI(who) + '></img>');
```

The error message is that 'who' is not defined.</br>

When I add `/api/action.php?act=getemail` to the main URL, it leaves the page blank and if I run the snippet in the console again, I get a 404 - Page not found.</br>

The second solution is a YouTube demo, where Burp Suite is intercepting the request of the main URL, add `/api/action.php?act=getemail` to the main URL and add `X-SAFEPROTECTION: enNlYW5vb2Zjb3Vyc2U=` to the header.</br>
This will return 200 OK, but no output, i.e. the flag, where, according to the demo the flag should appear.</br>
