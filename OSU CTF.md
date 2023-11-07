# OSU CTF

## Description
This write up will show how to solve the CTF for "OSU CTF"

**Difficulty:** Moderate

Hint: Always check the JS on the page for unlinked routes!

## Solution 1st flag
Task opens when going to page: Natasha Drew really wants to go to hacker camp but she doesn't have the grades. Hack into the OSUSEC student portal and give her all A's so she can go!

According to the hint, there is something in the JavaScript, but not at the login page.</br>
By trying SQLi `' OR 1=1;--` it is possible to login as `rhonda.daniels`, i.e. not admin.</br>
On the mainpage it shows the grades of the students.</br>

When looking at the JS-code in the main page, it shows 2 functions, but the first one has a link `/update-student/` followed with the student id.</br>
The student id's look like Base64.</br>
Get the id from a student and decode this in the terminal `echo "RmlubmxheV9DYXJy" | base64 -d` where it will return `Finnlay_Carr`, i.e. first, underscore, lastname.</br>

Now create the id for Natasha Drew with `echo "Natasha_Drew" | base64` and it will return the id: `TmF0YXNoYV9EcmV3Cg==`</br>

Go to `https://1a5dfd6275139e83f19250065a3a2604.ctf.hacker101.com/update-student/TmF0YXNoYV9EcmV3Cg==` where is Natasha_Drew converted to Base64.</br>
This will give an incorrect student id.</br>

It turns out that base64 from the terminal is converting it slightly different than with cyberchef.io for example: `TmF0YXNoYV9EcmV3`

Now go to `https://1a5dfd6275139e83f19250065a3a2604.ctf.hacker101.com/update-student/TmF0YXNoYV9EcmV3`, which will open the page to update the grades.</br>
Change all the grades to A and click on Update Record.</br>
Natasha's grades are updated and the flag is presented.
