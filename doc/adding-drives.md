Adding drives
-------------

Find the id of the drive:
```sh
sudo blkid
```

For this example we assume the drive is formatted in ext4. You should see something like this: `/dev/sdx1: LABEL="MyDrive" UUID="xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx" TYPE="ext4" PARTUUID="xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"`.

First we mount that drive on the host
```sh
sudo mkdir /mnt/MyDrive
sudo editor /etc/fstab
```

Add a line with the correct uuid of the drive.
```
UUID=xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx /mnt/MyDrive  ext4    errors=remount-ro 0       1
```

Then mount it.
```sh
sudo mount /mnt/MyDrive
```
