---
layout: post
title: "Fabric 1.1: Deploy a Fabric Composer Network on Ubuntu 16.04"
subtitle: "... there will be change"
date: 2018-02-10
categories: go
status: publish
---

Fabric Composer makes it incredibly easy to prototype a blockchain network, generate the RESTful API, and even creating the Angular front-end. The tutorial will guide you through those three components of Fabric Composer.


## Install required libraries
```
apt-get update
apt-get upgrade -y
apt-get install build-essential libssl-dev python2.7 unzip npm -y
ln -s `which python2.7` /usr/bin/python
```
Install NPM and NodeJS

```
sudo npm install n -g
sudo n latest
```

```
npm config set unsafe-perm true
npm install -g composer-cli@next composer-rest-server@next composer-playground@next
```

## Install Docker and Docker Composer
Docker 
```
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
sudo apt-get update
sudo apt-get install -y docker-ce
```

Docker composer 
```
sudo curl -L https://github.com/docker/compose/releases/download/1.18.0/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose

sudo chmod +x /usr/local/bin/docker-compose

docker-compose --version
```


From this point we can follow the Deploying a Fabric Composer Network. 
The main problem we'll encounter is the Yeoman generator. 