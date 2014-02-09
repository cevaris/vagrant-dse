# -*- mode: ruby -*-
# vi: set ft=ruby :

require 'json'
require 'erb'

key = File.new("#{Dir.home}/.ssh/vagrant-keys/id_rsa.pub").read()
credentials = JSON.parse(File.new("./.credentials").read())
provision_script = 


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
    node.vm.provision :shell, :inline => $provision_script

  end



end