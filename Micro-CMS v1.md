# Micro-CMS v1

## Description
This write up will show how to solve the CTF for "Micro-CMS v1"

**Difficulty:** Easy

Hints are not being provided for multiple reasons:</br>
1. It is better to figure it out yourself and I don't want to give spoilers, especially if you don't need one
2. It is only possible to get a hint every once in a while, i.e. it is not possible to get all the hints one after another. This will take too much time to "wait" for all the hints

## Solution 1st flag
Click on Create a new page and give a title and description</br>
After submitting the new page, it will redirect to a URL ending with a number</br>
When changing the numbers from 1 - 30, most pages respond Not Found, 1 is forbidden and a few are OK</br>
Click on Edit Page and changes the number to the forbidden page 4, which will show a flag:

![alt](https://github.com/8r0wn13/hacker101_ctf/blob/main/images/Screenshot%20from%202023-08-19%2001-21-07.png?raw=true)

## Solution 2nd flag
There is also an SQL injection, when editing a page and add a ' at the end of the URL:
![alt](https://github.com/8r0wn13/hacker101_ctf/blob/main/images/Screenshot%20from%202023-08-19%2001-21-07.png?raw=true)

## Solution 3rd flag
When creating a new page or editing a new page, the title allows for XSS</br>
Add an alert script in the title and click on Save:</br>
`<script>alert(1);</script>`

Going back to the home page will trigger the script and provide a pop-up with the flag:

![alt](https://github.com/8r0wn13/hacker101_ctf/blob/main/images/Screenshot%20from%202023-09-05%2021-56-13.png?raw=true)

## Solution 4th flag
When creating a new page or editing an existing page, it states markdown is supported, but scripts are not</br>
This means a way needs to be found to include XSS in the markup</br>
When editing the Markdown Test page it shows an image from adorable kittens in the body and there is a button</br>
The button can trigger an event, hence we could do for example:</br>
`onclick=alert(1)`

On the screren nothing happens when clicked on Save nor the button itself, but check the source code, there is the flag in the Button attribute:</br>
![alt](https://github.com/8r0wn13/hacker101_ctf/blob/main/images/Screenshot%20from%202023-09-05%2022-08-44.png?raw=true)
