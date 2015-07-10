# Dockerhost

A `Vagrantfile` to host docker containers on your OSX machine in a Virtual Machine. 

## Why Vagrant over boot2docker?

It allows more flexibility then boot2docker, for instance setting up file sharing etc. File sharing is needed if you
want to run `PostgreSQL` for example in development mode. You need a data directory to persist state. 

This Vagranfile attaches a fixed ip to the VM: 10.2.0.10.

## Getting started

### 1. Download and install Vagrant

Go and grab it on the [Vagrant Download page][https://www.vagrantup.com/downloads.html]

### 2. Clone the repository

```
git clone git@github.com:sgillis/dockerhost.git
```

### 3. Start the VM

```
$ cd dockerhost
vagrant up
```

### 4. Bash config

Add the following 2 lines to your `.bashrc` or `.bash_profile` file. This sets the docker host to your VM.

```
# This instructs docker to run containers on the VM and route traffic to the VM
export DOCKER_HOST="tcp://10.2.0.10:4243"
alias route-docker='sudo route -n add -net 172.17.0.0 10.2.0.10'
```

After you've added these lines you'll have to source it again:
```
source ~/.bash_profile
```

### 5. Use Docker

By now your VM is up and running and your environment is configured to use the VM as docker host.

```
docker info
```

## Troubleshooting
When executing `docker ps` you might receive the following error (or similar):

```
An error occurred trying to connect: Get https://localhost:4243/v1.19/containers/json: tls: oversized record received with length 20527
```

Probably you've been using boot2docker. When you run `boot2docker init` it adds a few exports to your bash 
configuration. This conflicts with your vagrant config. Delete the following lines from your bash config:

```
export DOCKER_HOST=tcp://192.168.59.103:2376
export DOCKER_CERT_PATH=/Users/johndoe/.boot2docker/certs/boot2docker-vm
export DOCKER_TLS_VERIFY=1
```

To be safe, unset these exports:
```
unset DOCKER_CERT_PATH
unset DOCKER_TLS_VERIFY
```

Now source your bash config again: `source ~/.bash_profile`.
