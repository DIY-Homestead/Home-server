Installing VirtualBox
=====================

Find the most recent version of VirtualBox here: `https://download.virtualbox.org/virtualbox/`.

At the time of writing that is `5.2.12` with build number `122591`.

Replace the files in the examples below with the most recent version.

Fetch and install VirtualBox
----------------------------

```sh
wget https://download.virtualbox.org/virtualbox/5.2.12/virtualbox-5.2_5.2.12-122591~Ubuntu~bionic_amd64.deb

sudo dpkg -i virtualbox-5.2_5.2.10-122088~Ubuntu~bionic_amd64.deb
```

Fetch and install the extension pack
------------------------------------

```sh
wget https://download.virtualbox.org/virtualbox/5.2.12/Oracle_VM_VirtualBox_Extension_Pack-5.2.12-122591.vbox-extpack

sudo VBoxManage extpack install Oracle_VM_VirtualBox_Extension_Pack-5.2.12-122591.vbox-extpack
```

Install the helper scripts
--------------------------

```sh
apt install gimle-vbox
```

Enable autocompletion
---------------------
```sh
sudo wget https://raw.githubusercontent.com/gryf/vboxmanage-bash-completion/master/VBoxManage -O /etc/bash_completion.d/VBoxManage
exec bash
```

Stopping the VM's before shutdown / reboot
------------------------------------------

To stop all the VM's before a shutdown / reboot run `stopallvms`

Auto staring VM's at boot
-------------------------

```sh
crontab -e
```
Add a new line for each VM
```
@reboot vbox --start vmname
```
