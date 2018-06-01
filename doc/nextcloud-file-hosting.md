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


Storage location
----------------

How to connect a drive on the host, and share it with the vm for nextcloud to store files on that drive.

### Host configuration

See [adding drives](adding-drives.md) to add a new drive.

```sh
sudo apt install nfs-kernel-server
```

Add a folder for nextcloud on the drive
```sh
sudo mkdir /mnt/MyDrive/nextcloud
```

Then we can share it:
```sh
sudo editor /etc/exports
```
```
/mnt/Mydrive  ip.of.guest.vm(no_root_squash,rw)
```
And reload shares:
```sh
sudo exportfs -ra
```



### Guest (nextcloud vm) configuration

```sh
sudo apt install nfs-common
```

Mount the new disk and move the data to it
```sh
sudo mkdir /mnt/MyDrive
sudo editor /etc/fstab
```
```
hostnamefromshare:/mnt/MyDrive /mnt/MyDrive nfs rsize=8192,wsize=8192
```
Mount it, and copy the data over to it
```sh
mount /mnt/MyDrive
cp -r /var/snap/nextcloud/common/nextcloud/data/ /mnt/MyDrive/nextcloud/
```
Then we can delete the old data and remount the drive to the nextcloud data directory.
```sh
sudo editor /etc/fstab
```
Replace the previous record with
```
hostnamefromshare:/mnt/MyDrive/nextcloud/data /var/snap/nextcloud/common/nextcloud/data nfs rsize=8192,wsize=8192
```
```sh
rm -rf /var/snap/nextcloud/common/nextcloud/data/{*,.*}
sudo mount /var/snap/nextcloud/common/nextcloud/data/
```

Enable and disable nextcloud
----------------------------

```
sudo snap disable nextcloud
sudo snap enable nextcloud
```

Enable and disable maintenance mode
-----------------------------------

```sh
sudo nextcloud.occ maintenance:mode --on
sudo nextcloud.occ maintenance:mode --off
```

Made changed directly on the filesystem
---------------------------------------

If you have have made changed directly on the filesystem you can make nexcloud scan it to discover the changes.

```sh
sudo nextcloud.occ files:scan --all
```
