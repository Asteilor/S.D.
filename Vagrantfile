# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/focal64"
  config.vm.box_check_update = false
  (1..2).each do |i|
    config.vm.define "otus-vm-ubuntu#{i}" do |web|
      web.vm.hostname = "otus-vm-ubuntu-#{i}"
      web.vm.provider "virtualbox" do |vb|
        vb.name = "otus-vm-ubuntu#{i}"
        vb.cpus = "2"
        vb.memory = "8192"
      end
    end
  end     
         
 # config.vm.network "forwarded_port", guest: 80, host: 8080, host_ip:"127.0.0.1"
 # config.vm.network "private_network", ip:"192.168.33.10"
end

