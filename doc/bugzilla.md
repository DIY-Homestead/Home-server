Bugzilla
========

```sh
libmime-perl lynx-cur libtemplate-perl-doc

sudo apt install mariadb-server apache2 libappconfig-perl libdate-calc-perl libtemplate-perl build-essential libdatetime-timezone-perl libdatetime-perl libemail-sender-perl libemail-mime-perl libemail-mime-perl libdbi-perl libdbd-mysql-perl libcgi-pm-perl libmath-random-isaac-perl libmath-random-isaac-xs-perl libapache2-mod-perl2 libapache2-mod-perl2-dev libchart-perl libxml-perl libxml-twig-perl perlmagick libgd-graph-perl libtemplate-plugin-gd-perl libsoap-lite-perl libhtml-scrubber-perl libjson-rpc-perl libdaemon-generic-perl libtheschwartz-perl libtest-taint-perl libauthen-radius-perl libfile-slurp-perl libencode-detect-perl libmodule-build-perl libnet-ldap-perl libauthen-sasl-perl libfile-mimeinfo-perl libhtml-formattext-withlinks-perl libfile-which-perl libgd-dev libmysqlclient-dev graphviz python-sphinx rst2pdf
sudo a2enmod cgi expires headers rewrite
sudo su -
mariadb
```

```sql
create database bugzilla;
CREATE USER 'bugzilla'@'localhost' IDENTIFIED BY 'password';
GRANT ALL PRIVILEGES ON bugzilla.* TO 'bugzilla'@'localhost' IDENTIFIED BY 'password' WITH GRANT OPTION;
```

```sh
git clone --branch release-5.0-stable https://github.com/bugzilla/bugzilla.git
sudo editor /etc/apache2/sites-available/bugzilla.conf
```

```
<Directory /home/username/bugzilla/>
	Options Indexes FollowSymLinks
	AllowOverride None
	Require all granted
</Directory>

<VirtualHost *:80>
	ServerName localhost

	DocumentRoot /var/www/html
	Alias /bugzilla /home/username/bugzilla

	<Directory /home/username/bugzilla>
		AddHandler cgi-script .cgi .pl
		Options +Indexes +ExecCGI +FollowSymLinks
		DirectoryIndex index.cgi index.html
		AllowOverride All
	</Directory>
</VirtualHost>
```

```sh
sudo a2dissite 000-default.conf
sudo a2ensite bugzilla.conf
sudo service apache2 restart
sudo usermod -a -G www-data username
```

Sign out and in again
```sh
./checksetup.pl
editor localconfig
```

```
$webservergroup = 'www-data';
$db_name = 'bugzilla';
$db_user = 'bugzilla';
$db_pass = 'password';
```

```sh
./checksetup.pl
```

Sign in and enter `Administration` -> `Parameters`.

Select `LDAP`

* LDAPserver: `ldap://ldapserverhostname`
* LDAPbinddn: `cn=admin,dc=valhall:BIND_PASSWORD`
* LDAPBaseDN: `ou=users,dc=valhall`
* LDAPfilter: `(ou=ou=admin,dc=orgname,dc=valhall)`

Select `User Authentication`

* user_verify_class: Add `LDAP`
* createemailregexp: Blank

Select `Email`

* mail_delivery_method: `None`
