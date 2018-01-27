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

## 3. Reference the newley added vbox in the Vagrantfile

...Still need to add stuff here...

## 4. Docker commands

The main Docker commands you may need to know should be used while using root access. Two common ways to do this are to start a command you need elevated permissions with `sudo` another way is to "mask" yourself as root with `sudo su - root`. You will remain logged in as root with the second option until you type `exit`. All docker commands below make the assumption that you have taken care of needing elevated permissions with your chosen method.

### 4.1 Is docker running?

Is the daemon running and the docker engine up:
```systemctl status docker```

A  respnse showing the engine is running:
```
root@localhost ~]# systemctl status docker
● docker.service - Docker Application Container Engine
   Loaded: loaded (/usr/lib/systemd/system/docker.service; enabled; vendor preset: disabled)
   Active: active (running) since Sat 2018-01-27 18:02:08 UTC; 4min 29s ago
     Docs: http://docs.docker.com
 Main PID: 887 (dockerd-current)
   Memory: 6.0M
   CGroup: /system.slice/docker.service
           ├─ 887 /usr/bin/dockerd-current --add-runtime docker-runc=/usr/libexec/docker/docker-runc-current --default-runtime=docker-runc --exec-opt native.cgroupdriver=systemd -...
           ├─ 905 /usr/bin/docker-containerd-current -l unix:///var/run/docker/libcontainerd/docker-containerd.sock --shim docker-containerd-shim --metrics-interval=0 --start-time...
           └─1159 /usr/bin/docker-containerd-shim-current 9cb81018f1af190446bd4e0d73fe9e76bfb162cbcdbded1ae50e73ed5cf5a313 /var/run/docker/libcontainerd/9cb81018f1af190446bd4e0d73...
```
### 4.2 What containers are currently running?

Check what containers are currently running

```docker ps```

Common response showing mysql running:
```
[root@localhost ~]# docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS               NAMES
9cb81018f1af        mysql:5.6.39        "docker-entrypoint.sh"   10 minutes ago      Up 2 minutes                            mysql5.6.39
```

### 4.3 Learn more about a specific container

Maybe you want to learn some stuff about a container:

```docker inspect mysql5.6.39```

common response:

```
[root@localhost ~]# docker inspect mysql5.6.39 
[
    {
        "Id": "9cb81018f1af190446bd4e0d73fe9e76bfb162cbcdbded1ae50e73ed5cf5a313",
        "Created": "2018-01-27T17:54:22.859113996Z",
        "Path": "docker-entrypoint.sh",
        "Args": [
            "mysqld"
        ],
        "State": {
            "Status": "running",
            "Running": true,
            "Paused": false,
            "Restarting": false,
            "OOMKilled": false,
            "Dead": false,
            "Pid": 1172,
            "ExitCode": 0,
            "Error": "",
            "StartedAt": "2018-01-27T18:02:08.55058622Z",
            "FinishedAt": "2018-01-27T18:01:30.692868146Z"
        },
        "Image": "sha256:f8418f2b6a58ac02044fb95b0b4dedf2d04ce5a42fc3bb5cabb1cea24d1269df",
        "ResolvConfPath": "/var/lib/docker/containers/9cb81018f1af190446bd4e0d73fe9e76bfb162cbcdbded1ae50e73ed5cf5a313/resolv.conf",
        "HostnamePath": "/var/lib/docker/containers/9cb81018f1af190446bd4e0d73fe9e76bfb162cbcdbded1ae50e73ed5cf5a313/hostname",
        "HostsPath": "/var/lib/docker/containers/9cb81018f1af190446bd4e0d73fe9e76bfb162cbcdbded1ae50e73ed5cf5a313/hosts",
        "LogPath": "",
        "Name": "/mysql5.6.39",
        "RestartCount": 0,
        "Driver": "devicemapper",
        "MountLabel": "",
        "ProcessLabel": "",
        "AppArmorProfile": "",
        "ExecIDs": null,
        "HostConfig": {
            "Binds": [
                "/data:/var/lib/mysql"
            ],
            "ContainerIDFile": "",
            "LogConfig": {
                "Type": "journald",
                "Config": {}
            },
            "NetworkMode": "host",
            "PortBindings": {
                "3306/tcp": [
                    {
                        "HostIp": "",
                        "HostPort": "3306"
                    }
                ]
            },
            "RestartPolicy": {
                "Name": "unless-stopped",
                "MaximumRetryCount": 0
            },
            "AutoRemove": false,
            "VolumeDriver": "",
            "VolumesFrom": null,
            "CapAdd": null,
            "CapDrop": null,
            "Dns": [],
            "DnsOptions": [],
            "DnsSearch": [],
            "ExtraHosts": null,
            "GroupAdd": null,
            "IpcMode": "",
            "Cgroup": "",
            "Links": null,
            "OomScoreAdj": 0,
            "PidMode": "",
            "Privileged": false,
            "PublishAllPorts": false,
            "ReadonlyRootfs": false,
            "SecurityOpt": null,
            "UTSMode": "",
            "UsernsMode": "",
            "ShmSize": 67108864,
            "Runtime": "docker-runc",
            "ConsoleSize": [
                0,
                0
            ],
            "Isolation": "",
            "CpuShares": 0,
            "Memory": 0,
            "CgroupParent": "",
            "BlkioWeight": 0,
            "BlkioWeightDevice": null,
            "BlkioDeviceReadBps": null,
            "BlkioDeviceWriteBps": null,
            "BlkioDeviceReadIOps": null,
            "BlkioDeviceWriteIOps": null,
            "CpuPeriod": 0,
            "CpuQuota": 0,
            "CpusetCpus": "",
            "CpusetMems": "",
            "Devices": [],
            "DiskQuota": 0,
            "KernelMemory": 0,
            "MemoryReservation": 0,
            "MemorySwap": 0,
            "MemorySwappiness": -1,
            "OomKillDisable": false,
            "PidsLimit": 0,
            "Ulimits": null,
            "CpuCount": 0,
            "CpuPercent": 0,
            "IOMaximumIOps": 0,
            "IOMaximumBandwidth": 0
        },
        "GraphDriver": {
            "Name": "devicemapper",
            "Data": {
                "DeviceId": "13",
                "DeviceName": "docker-253:0-34330234-8ff3fd6c3483ae5ec73fc823ac1c92e543494d924aa12a496e227a58ea2a733d",
                "DeviceSize": "10737418240"
            }
        },
        "Mounts": [
            {
                "Source": "/data",
                "Destination": "/var/lib/mysql",
                "Mode": "",
                "RW": true,
                "Propagation": "rprivate"
            }
        ],
        "Config": {
            "Hostname": "localhost.localdomain",
            "Domainname": "",
            "User": "",
            "AttachStdin": false,
            "AttachStdout": false,
            "AttachStderr": false,
            "ExposedPorts": {
                "3306/tcp": {}
            },
            "Tty": false,
            "OpenStdin": false,
            "StdinOnce": false,
            "Env": [
                "MYSQL_ROOT_PASSWORD=password",
                "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin",
                "GOSU_VERSION=1.7",
                "MYSQL_MAJOR=5.6",
                "MYSQL_VERSION=5.6.39-1debian8"
            ],
            "Cmd": [
                "mysqld"
            ],
            "Image": "mysql:5.6.39",
            "Volumes": {
                "/var/lib/mysql": {}
            },
            "WorkingDir": "",
            "Entrypoint": [
                "docker-entrypoint.sh"
            ],
            "OnBuild": null,
            "Labels": {}
        },
        "NetworkSettings": {
            "Bridge": "",
            "SandboxID": "d492a22fadb24d3cf287b4eab2407ea7da0cec7078d014422a2f1621b8fd082c",
            "HairpinMode": false,
            "LinkLocalIPv6Address": "",
            "LinkLocalIPv6PrefixLen": 0,
            "Ports": {},
            "SandboxKey": "/var/run/docker/netns/default",
            "SecondaryIPAddresses": null,
            "SecondaryIPv6Addresses": null,
            "EndpointID": "",
            "Gateway": "",
            "GlobalIPv6Address": "",
            "GlobalIPv6PrefixLen": 0,
            "IPAddress": "",
            "IPPrefixLen": 0,
            "IPv6Gateway": "",
            "MacAddress": "",
            "Networks": {
                "host": {
                    "IPAMConfig": null,
                    "Links": null,
                    "Aliases": null,
                    "NetworkID": "782074601e9613b1b15b25f5eb1c62ff32b56bf0d712eda46b2f8ae46981430b",
                    "EndpointID": "ec02c16856d31096aea04a0483fa40f07c63d9b83291583cde3b22effed10c4a",
                    "Gateway": "",
                    "IPAddress": "",
                    "IPPrefixLen": 0,
                    "IPv6Gateway": "",
                    "GlobalIPv6Address": "",
                    "GlobalIPv6PrefixLen": 0,
                    "MacAddress": ""
                }
            }
        }
    }
]
```

### 4.4 Docker Container Data Directory

An important part to take note of from the above inspect is:

```
"Binds": [
            "/data:/var/lib/mysql"
        ],
```
This tells us that the container is saving the database files to the `/data` directory inside the VM. Take care to avoid modifying any of these files as you can corrupt the database. The reason this is done is if the container crashes, the data inside the databse will pursist and be available upon the next boot up of the database.