# System setup documentation

## 1. Virtualbox/ Vagrant Box additions
After installing Vagrant, we need to install a couple plugins 

### 1.1 Virtualbox plugins

#### 1.1.1 Guest Additions Plugin
`vagrant plugin install vagrant-vbguest`

#### 1.1.2 Host Manager Plugin
`vagrant plugin install vagrant-hostmanager`

### 1.2 Add local vbox file to Vagrant

The syntax for adding a local vbox to vagrant:

```vagrant box add [options] <name, url, or path>```

#### 1.2.1 Windows

#### This will add the vbox you need with windows:
*(D:/VagrantBoxes/ is a made up directory, modify this to where you have stored the vbox file)*

**The / is typically used in linux and may seem incorrect for windows. Vagrant expects the directory references in a linux like format**

```vagrant box add ubuntu-16.04/oodlesOfMoodles file:///D:/VagrantBoxes/oodlesOfMoodles.box```

#### 1.2.2 Linux

##### This will add the vbox you need with linux:
*(/data/VagrantBoxes/ is a made up directory, modify this to where you have stored the vbox file)*

```vagrant box add ubuntu-16.04/oodlesOfMoodles /data/VagrantBoxes/oodlesOfMoodles.box```

#### 1.2.3 Vagrant has added the box to Virtualbox

This is what that command should yeild for you, when entered correctly

```
yeep@derp Vagrant]$ vagrant box add ubuntu-16.04/oodlesOfMoodles oodlesOfMoodles.box
==> box: Box file was not detected as metadata. Adding it directly...
==> box: Adding box 'ubuntu-16.04/oodlesOfMoodles' (v0) for provider: 
    box: Unpacking necessary files from: file:///home/yeep/Vagrant/oodlesOfMoodles.box
==> box: Successfully added box 'ubuntu-16.04/oodlesOfMoodles' (v0) for 'virtualbox'!
```
#### 1.2.4 Did that work?

Check to see if Vagrant sees your new box as a viable option

`vagrant box list`

```
[yeep@derp Vagrant]$ vagrant box list
centos/7                      (virtualbox, 1801.02)
ubuntu-16.04/oodlesOfMoodles  (virtualbox, 0)
```
### 1.3 Download the source code for the project
1. Create new Directory for your vagrant box and repo files
2. Inside this directory the structure will be like 
```
.
+-- Vagrantfile
+-- www
|   +-- moodle
|       +-- <Moodle Git repo files here>
|   +-- core-moodle
|       +-- <Moodle Git repo files here>
```
3. Clone the Moodle Repo into the moodle directory (command run from inside the moodle directory) 

    `git clone https://github.com/OodlesOfMoodles/moodle.git .`

4. OPTIONAL - Clone the Moodle Repo into 'core-moodle' so you have an unmodified working version to compare with your changed files
5. Get you favorite beverage and kick you feet up, this is a large download
6. Once download is complete, go to 1.4

### 1.4 Setting up your Vargrant box

1.  `vagrant init` will create a new file called "Vagrantfile" for you
2. Edit the new Vagrantfile
   1. Highlight all lines in the file and delete
   2. Paste in the Vagrantfile contents from the setup repo for the file that matches your OS
3. `vagrant up`

## 2 Databases

### 2.1 MySQL

You can connect to you virtual machine from the outside if you choose with a 3rd party mysql client like mysql workbench

* Host: 0.0.0.0
* Username: root
* Password: root

From within the virtual machine:

    > vagrant ssh
    > mysql -u root -p
    Enter password: [Type in root]

#### 2.1.1 Create Database in mySQL
*Two databases have already been created in mySQL, they are named: "moodle" and "moodle-core"*

Connect to MySQL using one of the methods mentioned above, then execute the following SQL:

    CREATE DATABASE `database-name` DEFAULT CHARACTER SET utf8 COLLATE utf8_general_ci;

Replace `database-name` with the database name you would like to create.


### 2.2 PostgreSQL

To connect to the default database to install Moodle use the following connection details inside config.php:

    $CFG->dbtype    = 'pgsql';
    $CFG->dblibrary = 'native';
    $CFG->dbhost    = '0.0.0.0';
    $CFG->dbname    = 'postgres';
    $CFG->dbuser    = 'postgres';
    $CFG->dbpass    = 'root';

To create a new database:

    > psql -c "create database database_name;" -U postgres template1

To access Postresql from the CLI:

    > vagrant ssh
    > psql -U postgres

## 3 Data Directory
Moodle requires a "data directory" for file uploads to be saved in.
The Moodle data directories can be created under the following directories:

* `/srv/moodledata/`
* `/vagrant/moodledata/`

The `/srv/moodledata` location is in the virtual machine itself.  This is generally
faster and since they are stored in the virtual machine, they are deleted if the
machine is ever destroyed.  This can be good or bad depending on how you like to manage things.

The `/vagrant/moodledata/` location is in the NFS mount.  This is generally slower,
but it makes the data directories accessible to your host machine and they are not
deleted if the virtual machine is ever destroyed.

Example of how to create a new one, use the following commands:

    > vagrant ssh
    > mkdir /srv/moodledata/dirname
    > chmod 777 /srv/moodledata/dirname

Replace `dirname` with the directory name you would like to create.

## 4 Run Moodle

### 4.1 Download source code
*This should already be done from step 1*
First, clone the Moodle repository into `www/moodle` directory.

`git clone git://github.com/moodle/moodle.git www/moodle`

### 4.2 Create A Database
Next, create a database and data directory for the site.  Follow
the steps in above sections to do this.

### 4.3 Use the web based Moodle installer
Lastly, go to [moodle.test](http://moodle.test) in your browser and follow the instructions to install Moodle.

Note: you can install a second Moodle site into `www/core-moodle`
and access it via [core-moodle.test](http://core-moodle.test). For example,
you may want to install your customized version of Moodle in `www/moodle`
and install the core version of Moodle in `www/core-moodle`.

Also note: the site configs for these directories are in `/etc/nginx/sites-enabled`
where you can add new sites or modify existing ones.

## 5 How to use XDebug

You must install a XDebug extension in your browser and turn on debugging in the browser.
Then in an IDE like PHPStorm, you can listen for debug connections.

To profile a page, do the following:

1. Add XDEBUG_PROFILE=1 to your URL.
2. Refresh/rerun the page to generate the profile.


## 6 Vagrant box interaction

### 6.1 SSH connection

To enter the Vagrant box via ssh connection simply type
`vagrant ssh`
in the directory where you ran the `vagrant up` command

To leave the ssh connection type `exit` while inside the box. 

### 6.2 Shut down

To shut down the vagrant box type:
`vagrant halt`

### 6.3 Destroy
** Note: this will not delete your shared directory (where the source code is) but it will delete all data in your current database and require a reinstall of moodle**

Sometimes your environment gets so out of control that you just need to start over. Go to the directory where your vagrant box is running and type:
`vagrant destory`
Your Vagrantfile should still exist, and you should be able to start over with a `vagrant up` (no `vagrant init` needed if the Vagrantfile persists)

