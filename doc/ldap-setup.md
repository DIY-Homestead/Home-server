Ldap setup
==========

This service provides authentication, and base records for humans, animals, locations, companies and so forth. This includes things like address, phone number, email, etc. Records can also contain tags and groups, but does not necessarily prefered to handle rights management. (authentication ≠ rights).

One could say that the LDAP is the root of your system, if you have a user in ldap, all other services like login and such could be configured to use the same login. When setting up websites and services that require authentication they will all query the same LDAP and could also be set up as a signle sig on point later.

The layout of this directory can be done in so many different ways. I do not claim to be an expert in this, and the suggested layout below is based on my limited knowledge. However, for a homestead server, it should be quite sufficient.

Installation
------------

```sh
sudo apt install slapd ldap-utils
sudo dpkg-reconfigure slapd
```

When configuring the initial database you will be asked some questions. I normally use a versitile name for my root dc, like "valhall" or something. Then I can create sub dc for anything later without them being directly tied to a more precise value.
The password will be for the admin user, and is used for configuration.

Configuration
-------------

For configuration, I normally use [Apache Directory Studio](http://directory.apache.org/studio/).

### Connecting ###

Choose new connection:
* Connection name: Anything you like, eg `Valhall`.
* Hostname: The hostname of the server where you installed ldap.
* Port: 389
Authenticate
* Bind DN or user: `cn=admin,dc=valhall`
Bind password: Your chosen admin password

### Creating a directory for the users ###

* Right click `dc=valhall` and choose `New` > `New Entry…`
* Choose `Create entry from scratch`
* Add `organizationalUnit`
* RDN: `ou` > `users`

### Creating your root (Organization) ###

* Right click `dc=valhall` and choose `New` > `New Entry…`
* Choose `Create entry from scratch`
* Add `organization`
* RDN: `dc` > `orgname`
* `o` > `Organization name`

### Creating a group ###

Contacts, Services, Administrators, Owners, Employees could be created as a group under your `dc=orgname` directory. Common groups refered to in this doc: `contacts`, `services`.

* Right click `dc=orgname` and choose `New` > `New Entry…`
* Choose `Create entry from scratch`
* Add `organizationalUnit`
* RDN: `ou` > `groupname`

### Creating an employee / admin / etc ###

* Right click `dc=orgname` and choose `New` > `New Entry…`
* Choose `Create entry from scratch`
* Add `inetOrgPerson`
* Add `posixAccount`
* RDN: `cn` > `first.lastname`
* sn: `Lastname`
* uidNumber: `1001` (increase by one for each user).
* gidNumber: `1001` (increase by one for each user).
* uid: `username`
* homeDirectory: `/home/username`

Add new attributes:
* givenName: `First Names`
* mail: `email@example.com`
* telephoneNumber: `123456789`
* userPassword: `CRYPT-SHA-512 hashed password`
* ou: `ou=groupname,dc=orgname,dc=valhall` (Right click the group(s) you want want the user to be a part of, choose properties, and copy the DN value from there.

### Creating a service ###

Services is anything that does not fall into the category above, but that still needs a way to authenticate / retrieve a valid user list / etc. Like a wiki, cloud service and more.

* Right click `ou=services` and choose `New` > `New Entry…`
* Choose `Create entry from scratch`
* Add `person`
* RDN: `cn` > `servicename`
* sn: `Service name`
* userPassword: `CRYPT-SHA-512 hashed password`

### Creating a contact ###

* Right click `ou=contacts` and choose `New` > `New Entry…`
* Choose `Create entry from scratch`
* Add `inetOrgPerson`
* RDN: `cn` > `first.lastname`
* sn: `Lastname`

Add new attributes:
* givenName: `First Names`
* mail: `email@example.com`
* telephoneNumber: `123456789`
