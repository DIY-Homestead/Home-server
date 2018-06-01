VM: ubuntu server installation and setup
========================================

Fetching a iso image
--------------------

```sh
wget http://releases.ubuntu.com/18.04/ubuntu-18.04-live-server-amd64.iso
```

Creating a new server
---------------------

```sh
createvm --name vmname --iso ubuntu-18.04-live-server-amd64.iso
```

Enable RDP and start the VM
```sh
vbox --rdp vmname 51001
vbox --start vmname
```

You can now RDP into the newly created VM at port 51001 to [continue the installation process](installing-ubuntu-server.md). Since we will set up ldap authenticatication on this server, it is adviced to only setup a local admin account during installation. Eg, Name: `Administrator`, username: `admin`.

Once the installation is done and the VM is powered off, you can free the RDP port again, and take a snapshot of it.
```sh
vbox --rdp vmname
vboxmanage snapshot vmname take "Initial"
```

Setting up the vm
-----------------

```sh
sudo apt install ldap-auth-client
sudo auth-client-config -t nss -p lac_ldap
```

* Server name: ldap://ldapservername
* Distinguished name: `dc=valhall`
* Version: `3`
* Make local root Database admin: `yes`
* Does the LDAP database require login?: `no`
* LDAP account for root: `cn=admin,dc=valhall`
* LDAP root account password: Your ldap admin password.

Enable user directories
```sh
sudo editor /usr/share/pam-configs/ldap_mkhomedir
```
```
Name: activate mkhomedir
Default: yes
Priority: 900
Session-Type: Additional
Session:
        required                        pam_mkhomedir.so umask=0022 skel=/etc/skel
```

If you want to limit user access to members of a speciffic group
```sh
sudo editor /etc/ldap.conf
```

Change `groupname` and `orgname` to your values.
```
nss_base_passwd ou=users,dc=valhall?one?ou=ou=groupname,dc=orgname,dc=valhall
```

Verify that user has access
```sh
getent passwd username
```

Promoting a ldap user to sudo
-----------------------------

To give an ldap user sudo rights, simply add the sudo group to it

```sh
sudo usermod -aG sudo username
```

Removing a vm
-------------

Remove the vm from VirtualBox
```sh
vboxmanage unregistervm vmname
```

Delete the configuration files for the vm
```sh
rm -rf VirtualBox\ VMs/vmname
```

Delete the HDD
```sh
rm VirtualBox\ Disks/vmname.vdi
```
