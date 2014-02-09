# -*- mode: ruby -*-
# vi: set ft=ruby :

require 'json'

key = File.new("#{Dir.home}/.ssh/vagrant-keys/id_rsa.pub").read()
credentials = JSON.parse(File.new("./.credentials").read())
provision_script = %{
#!/bin/bash
echo "Adding authorized_keys to root user"
sudo mkdir -p /root/.ssh/
sudo echo "#{key}" >> /root/.ssh/authorized_keys
echo "Done adding authorized_keys"

echo "Adding authorized_keys to vagrant user"
mkdir -p .ssh/
echo "#{key}" >> .ssh/authorized_keys
echo "Done adding authorized_keys"


echo "Updtaing..."
sudo apt-get update
echo "... done updating"


echo "Installing Java 7"
sudo apt-get install openjdk-7-jdk openjdk-7-jre curl -y
echo "... done installing Java 7"


if grep "@debian.datastax.com/enterprise" /etc/apt/sources.list.d/datastax.sources.list >/dev/null; then
  echo "DSE apt sources already added"
else
  echo "Adding DSE apt sources"
  echo "deb http://#{credentials['username']}:#{credentials['password']}@debian.datastax.com/enterprise stable main" | \
  sudo tee -a /etc/apt/sources.list.d/datastax.sources.list
fi

curl -L https://debian.datastax.com/debian/repo_key | sudo apt-key add -
echo "... done adding DSE apt sources"

echo "Updtaing..."
sudo apt-get update
echo "... done updating"

echo "Installing DSE"
sudo apt-get install dse-full opscenter -y
echo "... done installing DSE"

echo "Script done..."
}


NAME = 'dse'
IP_ADDRESS = '192.168.3.100'

Vagrant.configure("2") do |config|



  config.vm.define NAME do |node|
    
    node.vm.box = "precise64"
    node.vm.box_url = "http://cloud-images.ubuntu.com/vagrant/precise/current/precise-server-cloudimg-amd64-vagrant-disk1.box"
    # node.vm.box = "centos6"
    # node.vm.box_url = "https://github.com/2creatives/vagrant-centos/releases/download/v0.1.0/centos64-x86_64-20131030.box"

    node.vm.provider :virtualbox do |v|
      v.name = NAME
      v.customize ["modifyvm", :id, "--memory", "2048"]
    end

    node.vm.network :private_network, ip: IP_ADDRESS
    node.vm.hostname = NAME
    node.vm.provision :shell, :inline => provision_script

  end



end