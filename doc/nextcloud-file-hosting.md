Nextcloud
=========

Installation
------------

Create a new virtual machine, and install nexcloud on it, and change the configuration to work with running in a subfolder

```sh
sudo snap install nextcloud
sudo editor /var/snap/nextcloud/current/nextcloud/config/config.php
```

Add to the config
```
'overwritewebroot' => '/nextcloud'
```

Go to the host, and add [nginx configuration](nginx-proxy-setup.md), add certificate, and add a location block for the proxy, eg:
```
	location ~ ^/nextcloud {
		rewrite ^/nextcloud(/.*)$ $1 break;
		proxy_pass http://servername:80;
		…
```

Visit https://homstead.exemple.com/nextcloud/ to finish the setup.

Configuration
-------------

By default LDAP is not enabled. Go to `Apps` -> `Disabled apps` and enable `LDAP user and group backend`.

Configure LDAP by entering `Settings` -> `LDAP / AD integration`.

### Server tab

* Host: Your hosts machines address
Detect port

* User DN: `cn=admin,dc=valhall`
* Password: Your password

Save Credentials

Detect Base DN

Test Base DN

Continue

### Users tab

Edit LDAP Query:
`(&(objectclass=inetOrgPerson)(ou=ou=admin,dc=orgname,dc=valhall))`

Continue

### Login Attributs

* LDAP / AD Username: Checked
* LDAP / AD Email Address: Checked
* Other Attributes: `uid`

Continue

Done.

Go to `Users` and click `Settings`: Select your default quota for users.

Apps
----

There are many apps available, here is a selected few. Some might appear in multiple categories.

### Show RAW image files from cameras

* Files -> Camera RAW Previews

### Create nice charts and schemas

* Files -> Draw.io
* Integration -> Draw.io
* Organization -> Draw.io

### Warn users when close to quota limits

* Files -> Quota warning
* Monitoring -> Quota warning

### Edit markdown files

* Office & text -> Markdown Editor

### View PDF files

* Files -> PDF viewer
* Office & text -> PDF viewer
