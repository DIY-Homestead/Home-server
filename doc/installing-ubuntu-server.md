Installing Ubuntu server
========================

Getting started
---------------

All that is needed is a normal [ubuntu server](https://www.ubuntu.com/download/server) install. If you want the server to talk the same language as almost all guides on the internet, install it in English, but choose your country's keyboard. Additional language support is added later. Once the OS is installed make sure it is up to date and has remote access.
```sh
sudo apt update && sudo apt dist-upgrade && sudo apt autoremove
sudo apt install ssh
```
Once complete there is no more need for a monitor or keyboard attached to the machine, it can go down in the basement :)

From this point on, you can ssh into the machine.

Setting up locale
-----------------

Setting up a locale is pretty easy, the example below is for adding Norwegian, but you add the locale that is right for you:
```sh
sudo locale-gen nb_NO.UTF-8
```

Once you added the locales you want, even if that was none, you should reconfigure. (This will remove those annoying messages that locale is not defined).

```sh
sudo dpkg-reconfigure locales -f noninteractive
```

To see what locales you have installed
```sh
locale -a
```

Setting up additional packages
------------------------------

```sh
sudo add-apt-repository ppa:tux-/gimle
sudo apt update && sudo apt install gimle-core
```

Done
----

That was it for the basic server setup. And also all that was added to this repo atm. From here on we will try to create small .deb or snap packages to semi automate setups and installations.
