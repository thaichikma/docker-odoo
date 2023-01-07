# Quick install

Installing Odoo 16 with one command.

(Supports multiple Odoo instances on one server)

Install [docker](https://docs.docker.com/get-docker/) and [docker-compose](https://docs.docker.com/compose/install/) yourself, then run:
``` bash
sudo apt update && apt upgrade
sudo apt install docker docker-compose -y
```
Then
``` bash
curl -s https://raw.githubusercontent.com/thaichikma/docker-odoo/16.0/run.sh | sudo bash -s odoo-one 10012 20012
```

to set up first Odoo instance @ `localhost:10012` (default master password: `Masterpass`)

and

``` bash
curl -s https://raw.githubusercontent.com/thaichikma/docker-odoo/16.0/run.sh | sudo bash -s odoo-two 11012 21012
```

to set up another Odoo instance @ `localhost:11012` (default master password: `Masterpass`)

Some arguments:
* First argument (**odoo-one**): Odoo deploy folder
* Second argument (**10012**): Odoo port
* Third argument (**20012**): live chat port

If `curl` is not found, install it:

``` bash
$ sudo apt-get install curl
# or
$ sudo yum install curl
```

# Usage

Start the container:
``` sh
docker-compose up
```

* Then open `localhost:10012` to access Odoo 12.0. If you want to start the server with a different port, change **10012** to another value in **docker-compose.yml**:

```
ports:
 - "10012:8069"
```

Run Odoo container in detached mode (be able to close terminal without stopping Odoo):

```
docker-compose up -d
```

**If you get the permission issue**, change the folder permission to make sure that the container is able to access the directory:

``` sh
$ git clone https://github.com/thaichikma/docker-odoo/$version
$ sudo chmod -R 777 addons
$ sudo chmod -R 777 etc
$ mkdir -p postgresql
$ sudo chmod -R 777 postgresql
```

Increase maximum number of files watching from 8192 (default) to **524288**. In order to avoid error when we run multiple Odoo instances. This is an *optional step*. These commands are for Ubuntu user:

```
$ if grep -qF "fs.inotify.max_user_watches" /etc/sysctl.conf; then echo $(grep -F "fs.inotify.max_user_watches" /etc/sysctl.conf); else echo "fs.inotify.max_user_watches = 524288" | sudo tee -a /etc/sysctl.conf; fi
$ sudo sysctl -p    # apply new config immediately
```

# Custom addons

The **addons/** folder contains custom addons. Just put your custom addons if you have any.

# Odoo configuration & log

* To change Odoo configuration, edit file: **etc/odoo.conf**.
* Log file: **etc/odoo-server.log**
* Default database password (**admin_passwd**) is `Masterpass`, please change it @ [etc/odoo.conf#L55](/etc/odoo.conf#L55)

# Odoo container management

**Run Odoo**:

``` bash
docker-compose up -d
```

**Restart Odoo**:

``` bash
docker-compose restart
```

**Stop Odoo**:

``` bash
docker-compose down
```
**Upgrade pip and install lib paramiko**:
``` bash

pip install --upgrade pip

#install setuptools.

pip install setuptools==39.1.0

#install pyparser.

pip install pyparser==1.0
pip install pyparsing==2.1.0

#install cffi.

pip install cffi==1.11.5

#install cryptography.

pip install cryptography==2.2.2
pip install paramiko

```



**Docker commit**:

``` bash
docker commit [OPTIONS] CONTAINER [REPOSITORY[:TAG]]
```
**Docker push**:

``` bash
docker push [OPTIONS] NAME[:TAG]

docker docker push thaichikma/image
```
**Docker save**:

``` bash
docker save [OPTIONS] IMAGE [IMAGE...]

docker save image > image.zip
```
**Docker Load**:

``` bash
docker load [OPTIONS]

docker load < image.zip
```

More info: https://docs.docker.com/engine/reference/commandline/inspect/
# Examples:
$ **docker ps**

``` bash
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS              NAMES
c3f279d17e0a        ubuntu:12.04        /bin/bash           7 days ago          Up 25 hours                            desperate_dubinsky
197387f1b436        ubuntu:12.04        /bin/bash           7 days ago          Up 25 hours                            focused_hamilton
```
**docker commit c3f279d17e0a  svendowideit/testimage:version3**
``` bas
f5283438590d
```
$ **docker images**

``` bas
REPOSITORY                        TAG                 ID                  CREATED             SIZE
svendowideit/testimage            version3            f5283438590d        16 seconds ago      335.7 MB
```

# Live chat

In [docker-compose.yml#L21](docker-compose.yml#L21), we exposed port **20012** for live-chat on host.

Configuring **nginx** to activate live chat feature (in production):

``` conf
#...
server {
    #...
    location /longpolling/ {
        proxy_pass http://0.0.0.0:20012/longpolling/;
    }
    #...
}
#...
```
# **Docker Portainer**
* Install
``` bas
docker run -d -p 8000:8000 -p 9443:9443 --name portainer --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer-ce:latest
```
# docker-compose.yml

* odoo:16.0
* postgres:15

