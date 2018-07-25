Construct WebDAV server on Ubuntu
===
***Note:*** All commands below are executed under root by default. If you were required to have admin permission, try add `sudo` in front of that command so it's like `sudo [some command]`.

1: Configure Apache2:
---
Enable DAV and relative apache modules:
```
a2enmod dav_fs dav dav_lock
```
restart apache2 to get the above modules to run:
```shell
systemctl restart apache2
```

2: Create and configure Apache virtual machine directory:
---
Create a new directory for virtual machine:
```shell
mkdir /var/www/sync
chown www-data:www-data /var/www/sync
```
Create a new apache user (Remember the newly created account and password, which will be used later):
```shell
# This will prompt you to enter new password
htpasswd -c /var/www/me.dav [account name]
chown root:www-data /var/www/me.dav
chmod 640 /var/www/me.dav
```
Configure the virtual machine settings:
```shell
# you can also use whatever editor you like
vi /etc/apache2/sites-available/webdav.conf
```
Insert the follow configurations and exit
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

Create an alias in sites-enabled as well:
```
cd /etc/apache2/sites-enabled/
ln -s ../sites-available/webdav.conf webdav.conf
rm 000-default.conf
```

3: Use Cadaver to start service
---
Restart apache2 again, and use cadaver to login
```
service apache2 restart
apt-get install cadaver
cadaver http://127.0.0.1/webdav/
```

4: Lastly, configure your client
---
Based on different client the specific operations may vary, but mainly there are only 3 fields to fill before we finish:
```
URL: [(your ip)123.123.123.123]/webdav
account: [the one you created]
password: [the one you entered]
```
Note that we must put `/webdav` after the ip address, or otherwise the client will connect to the root directory by default.

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE1NzE5NjAxMjNdfQ==
-->