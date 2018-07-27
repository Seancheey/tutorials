***Note:*** All commands below are executed under ***root user*** by default. If you are not root user and are required to have admin permission,  add `sudo` in front of that command so it's like `sudo [some command]`.

1: Install Apache2:
---
```
apt-get update
apt-get install apache2
```

2: Configure Apache2:
---

Enable DAV and relative apache modules:
```
a2enmod dav_fs dav dav_lock
```
restart apache2 to get the above modules running:
```shell
systemctl restart apache2
```

3: Create user and virtual machine directory:
---
Create a new directory for virtual machine:
```shell
mkdir /var/www/sync
```
Create a new apache user (Remember the newly created account and password, which will be used later):
```shell
# This will prompt you to enter new password
htpasswd -c /var/www/me.dav [account name]
```

4: Create a WebDAV configuration:
---
Configure the virtual machine settings:
```shell
# you can also use whatever editor you like
vi /etc/apache2/sites-available/webdav.conf
```
Type `:set paste`, press `i` to enter INSERT mode, then paste the follow configurations, finally exit by press `esc` to exit INSERT mode, and type `:x`.
```apache2
<VirtualHost *:80>
	ServerAdmin webmaster@localhost
	DocumentRoot /var/www/sync/
	<Directory /var/www/sync/>
		Options Indexes MultiViews
		AllowOverride None
		Require all granted
	</Directory>
	Alias /webdav /var/www/sync
	<Location /webdav>
		DAV On
		AuthType Basic
		AuthName "webdav"
		AuthUserFile /var/www/me.dav
		Require valid-user
	</Location>
</VirtualHost>
```

Remove default site configuration and enable our webdav.conf, this removes the 000-default alias in /etc/apache2/sites-enabled/ and create webdav alias in the same directory:
```
a2dissite 000-default
a2ensite webdav
```

Restart to get everything done:
```
systemctl restart apache2
```

5: Lastly, configure your client
---
Based on different client the specific operations may vary, but mainly there are only 3 fields to fill before we finish:
```
URL: [(your ip)123.123.123.123]/webdav
account: [the one you created]
password: [the one you entered]
```

For linux users, you can use Cadaver to access WebDAV. 
Note that we must put `/webdav` after the ip address, or otherwise the client will connect to the root directory by default.
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTczMzU0MjcyMl19
-->