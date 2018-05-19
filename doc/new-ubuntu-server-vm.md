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

You can now RDP into the newly created VM at port 51001 to [continue the installation process](installing-ubuntu-server.md).

Once the installation is done and the VM is powered off, you can free the RDP port again
```sh
vbox --rdp vmname
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
