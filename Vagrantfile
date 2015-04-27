# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
    config.vm.define :docker do |docker|
        docker.vm.box = "ubuntu/trusty64"
        docker.vm.network "private_network", ip: "172.18.42.2"
        $script = <<SCRIPT
wget -q -O - https://get.docker.io/gpg | apt-key add -
echo deb http://get.docker.io/ubuntu docker main > /etc/apt/sources.list.d/docker.list
apt-get update -qq 
apt-get install -q -y --force-yes lxc-docker
usermod -a -G docker vagrant
# sed -e 's/DOCKER_OPTS=/DOCKER_OPTS=\"-H 0.0.0.0:4243\"/g' /etc/init/docker.conf > /vagrant/docker.conf.sed
# cp /vagrant/docker.conf.sed /etc/init/docker.conf
# rm -f /vagrant/docker.conf.sed
echo 'DOCKER_OPTS="-H 0.0.0.0:4243"' >> /etc/default/docker
service docker restart
SCRIPT
       docker.vm.provision :shell, :inline => $script
    end
end
