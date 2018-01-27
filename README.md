# System setup documentation

## 1. Virtualbox Guest Additions
After installing Vagrant, run the following command for vagrant to setup your vbguest additions. This is required to be able to sync to /var/www/html

`vagrant plugin install vagrant-vbguest`

## 2. Add local vbox file to Vagrant

The syntax for adding a local vbox to vagrant:

```vagrant box add [options] <name, url, or path>```

### 2.1 Windows

#### This will add the vbox you need with windows:
*(D:/VagrantBoxes/ is a made up directory, modify this to where you have stored the vbox file)*

**The / is typically used in linux and may seem incorrect for windows. Vagrant expects the directory references in a linux like format**

```vagrant box add centos7/oodlesOfMoodles file:///D:/VagrantBoxes/centos.oodles.box```

### 2.2 Linux

#### This will add the vbox you need with linux:
*(/data/VagrantBoxes/ is a made up directory, modify this to where you have stored the vbox file)*

```vagrant box add centos7/oodlesOfMoodles /data/VagrantBoxes/centos.oodles.box```

### 3. Reference the newley added vbox in the Vagrantfile
