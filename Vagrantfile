# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.hostname = 'docker.dev'

  config.vm.box = "phusion/ubuntu-12.04-amd64"
  config.vm.box_version = "2014.05.11"

  config.vm.provider "vmware_fusion" do |provider|
    provider.vmx['memsize'] = 2048
    provider.vmx['numvcpus'] = 4
  end

  config.vm.provider "virtualbox" do |v|
    config.vm.network "private_network", ip: "192.168.50.42"
  end

  # Share my entire home directory, so I can share various things with
  # individual docker containers.
  config.vm.synced_folder ENV['HOME'], "/Users/#{ENV['USER']}", type: "nfs"

  config.vm.provision :shell, inline: <<-SHELL
    sudo apt-key adv --keyserver hkp://p80.pool.sks-keyservers.net:80 --recv-keys 58118E89F3A912897C070ADBF76221572C52609D
    sudo apt-get update
    sudo apt-get install linux-image-generic-lts-trusty -y
    sudo apt-get install docker-engine -y
    sudo apt-get autoremove --purge -y

    adduser vagrant docker

    echo 'DOCKER_OPTS="-H tcp://0.0.0.0:2375 -H unix:///var/run/docker.sock"' >> /etc/default/docker
    restart docker
  SHELL
end
