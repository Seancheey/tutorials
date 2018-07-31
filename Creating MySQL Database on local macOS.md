1: Download MySQL
---
by default, macOS uses SQLite database. so you have to download MySQL by yourself. 

I strongly recommend using **home-brew** to install MySQL. You can also install many other packages like Python, jdk etc. with home-brew. So first we use automated script to install home-brew:
```shell
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

Then download MySQL....
```shell
brew install mysql
```
and start service
```
brew services start mysql
```
You'll often need to use above command to open a port at local  for incoming connection. It creates a sql service at localhost(127.0.0.1) on port 3306 by default. 

2: Connect to MySQL
---
Before using MySQL, I recommend you to change your password, but it is not necessary as long as you can ensure your machine's cyber security. 
```
mysqladmin -u[user] -p[old password] password
```

mysql command basic format: `mysql -u[user] -p[password] [optional database]
`. Notice that there is **no space between options and values**, otherwise the program my recognize your password as database name.

By default, mysql's admin user is root, and it has no password. So you can basically connect to the database by:
```
mysql -uroot
```


3: Create a new database
---
```
mysql>CREATE DATABASE [dbname];
mysql>use [dbname];
```

Then you can do whatever you want with it :/....
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTIxMzUwMTkzNDBdfQ==
-->