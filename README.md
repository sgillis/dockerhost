# Dockerhost

Virtual machine used for running a docker daemon. I decided to use this instead
of boot2docker because this allows more flexibility (for instance setting up
file sharing etc).

This attaches a fixed ip to your Virtual Machine, 10.2.0.10.

## Bash config

Add the following 2 lines to your `.bashrc` or `.bash_profile` file. This sets the docker host to your VM.

```
export DOCKER_HOST="tcp://10.2.0.10:4243"
alias route-docker='sudo route -n add -net 172.17.0.0 10.2.0.10'
```

After you've added these line you'll have to source it again:
```
source ~/.bash_profile
```
