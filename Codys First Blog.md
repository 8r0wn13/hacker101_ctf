# Photo Gallery

## Description
This write up will show how to solve the CTF for "Cody's First Blog"

**Difficulty:** Moderate

Hints are not being provided for multiple reasons:</br>
1. It is better to figure it out yourself and I don't want to give spoilers, especially if you don't need one
2. It is only possible to get a hint every once in a while, i.e. it is not possible to get all the hints one after another. This will take too much time to "wait" for all the hints

## Solution 1st flag
The web page states it is using PHP, hence test for PHP injection in the textbox:</br>
`<?php phpinfo(); ?>`

This will return the flag.</br>

## Solution 2nd flag
In the source code of the webpage there is a reference to an admin panel: `<!--<a href="?page=admin.auth.inc">Admin login</a>-->`</br>

admin.auth.inc is used to login, however, there is also a admin.inc page, to approve all the posts being made.</br>
On the bottom is the flag.</br>

## Solution 3rd flag
Create a comment with the following payload:
```
<?php readfile('index.php'); ?>
```

Approve it being the admin at the address `?page=admin.inc`

Go to ?page=http://127.0.0.1/index and open the source code.</br>
This will show the PHP source code of index.php.
