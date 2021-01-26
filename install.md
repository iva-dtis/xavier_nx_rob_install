# Post installation 

```bash
sudo apt update
sudo apt install gparted geeqie terminator git screen openssh-server tree htop 
sudo apt purge libreoffice*
sudo apt purge thunderbird
sudo apt autoremove
sudo apt upgrade
```

# Setup NVme disk

```
sudo mkdir /data
```


On gparted :

- create partition table on Nvme

- create 16Go of swap on nvme

- create a ext4 with all remaned space 

Get UUID on the created ext4 partition and add this line to /etc/fstab:

```
UUID=[theuuid]      /data   ext4    defaults        0       0 
```

idem get uuid of swap partition and add this lline to /etc/fstab :
```
UUID=bb269471-a627-461c-a2be-b760d84999f6	none	swap	sw	0	0
```

```
sudo mount /data
sudo chown $USER /data/
```


# Install OpenCV

# build ROS1 from source on Python3

# build ROS2 from source



