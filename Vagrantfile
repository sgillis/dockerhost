# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
    config.vm.define :docker do |docker|
        docker.vm.box = "ubuntu/trusty64"
        docker.vm.network "private_network", ip: "10.2.0.10", netmask: "255.255.0.0"
        docker.vm.provider :virtualbox do |vb|
            vb.customize ["modifyvm", :id, "--nicpromisc2", "allow-all"]
        end
        config.vm.synced_folder "/Users/sgillis", "/Users/sgillis",
            :nfs => true,
            :linux__nfs_options => ["no_root_squash"], :map_uid => 0, :map_gid => 0
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
