# -*- mode: ruby -*-
# vi: set ft=ruby :

require 'open3'
require 'fileutils'

Vagrant.configure("2") do |config|

config.vm.define "server" do |server|
  config.vm.box = 'almalinux/8'
  #config.vm.box_url = 'http://cloud.centos.org/centos/8/vagrant/x86_64/images/CentOS-8-Vagrant-8.4.2105-20210603.0.x86_64.vagrant-virtualbox.box'

  server.vm.host_name = 'server'
  server.vm.network :private_network, ip: "192.168.56.240"

  server.vm.provider "virtualbox" do |vb|
    vb.memory = "1024"
    vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
  end


  #server.vm.provision "shell",
    #name: "configuration",
    #path: "init.sh"
  end


  config.vm.define "client" do |client|
    client.vm.box = 'almalinux/8'
    client.vm.host_name = 'client'
    client.vm.network :private_network, ip: "192.168.56.241"
    client.vm.provider :virtualbox do |vb|
      vb.memory = "1024"
      vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
    end
  end


end
