# Grindyzer Installation

Perform the following steps starting from the directory containing the compressed application. Commands for installing packages assumes a debian based distribution. Adjust accordingly if necessary. 

## Install docker on the host.
- `apt update`
- `apt install apt-transport-https ca-certificates curl gnupg2 software-properties-common`
- `curl -fsSL https://download.docker.com/linux/debian/gpg | sudo apt-key add -`
- `add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/debian $(lsb_release -cs) stable"`
- `apt update`
- `install docker-ce` (See note below before running)


Note: 

The test server deployed to for testing on March 10th, 2020 required the following additional fixes for docker to run properly:
	
- `apt-get install docker-ce=17.09.1~ce-0~debian` in place of `apt-get install docker-ce`
- `mkdir /sys/fs/cgroup/systemd`
- `mount -t cgroup -o none,name=systemd cgroup /sys/fs/cgroup/systemd`

## Ensure docker is running.
	
Execute `docker run hello-world` expecting the output:
```
Hello from Docker!
This message shows that your installation appears to be working correctly.
```


## Install `docker-compose`. 

- `curl -L https://github.com/docker/compose/releases/download/1.22.0/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose`
- `chmod +x /usr/local/bin/docker-compose`


## Disable any existing services running on ports 80, 443, and 3306.

- `service httpd stop`
- `service mysql stop`

## Install the application to `/var/docker/`

- `mkdir /var/docker/`
- `tar -C /var/docker/ -xvf Grindyzer.tgz`

## Enable log rotation:

- `cp /var/docker/Grindyzer/scripts/logrotate.txt /etc/logrotate.d/docker`

## Initialize the docker images for the first time:

- `cd /var/docker/Grindyzer/docker/grindyzer-production`
- `docker-compose up`
