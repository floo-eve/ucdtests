# -*- mode: ruby -*-
# vi: set ft=ruby :

# README
#
# Getting Started:
# 1. vagrant plugin install vagrant-hostmanager
# 2. vagrant up
# 3. vagrant ssh
#
# This should put you at the control host
#  with access, by name, to other vms
Vagrant.configure(2) do |config|

config.vm.provider "virtualbox" do |v|
  v.memory = 2048
  v.cpus = 2
end


  config.hostmanager.enabled = true

  config.vm.box = "ubuntu/xenial64"

  config.vm.define "control", primary: true do |h|
    h.vm.network "private_network", ip: "192.168.135.10"
    h.vm.hostname = "control"
    h.vm.provision :shell, inline: 'command -v python > /dev/null || apt-get -y install python-minimal'
    h.vm.provision "ansible" do |ansible| 
      ansible.verbose = "v" 
      ansible.playbook = "apps.yml" 
    end 
  end

  config.vm.define "ucds" do |h|
    h.vm.network "private_network", ip: "192.168.135.11"
    h.vm.box = "centos/7"
    h.vm.hostname = "ucdserver"
    h.vm.network "forwarded_port", guest: 8443, host: 10443 
    h.vm.provision "ansible" do |ansible| 
      ansible.verbose = "v" 
      ansible.playbook = "install.yml" 
    end 
   
  end


  config.vm.define "app01", autostart=false do |h|
    h.vm.network "private_network", ip: "192.168.135.111"
    h.vm.box = "ubuntu/xenial64"
    h.vm.hostname = "app01"
    h.vm.provision :shell, inline: 'command -v python > /dev/null || apt-get -y install python-minimal'
    h.vm.provision "ansible" do |ansible| 
      ansible.verbose = "v" 
      ansible.playbook = "apps.yml" 
    end 
    
  end

  config.vm.define "app02", autostart=false do |h|
    h.vm.box = "ubuntu/xenial64"
    h.vm.network "private_network", ip: "192.168.135.112"
    h.vm.hostname = "app02"
    h.vm.provision :shell, inline: 'command -v python > /dev/null || apt-get -y install python-minimal'
    h.vm.provision "ansible" do |ansible| 
      ansible.verbose = "v" 
      ansible.playbook = "apps.yml" 
    end 
  end

  config.vm.define "db01", autostart=false do |h|
    h.vm.box = "ubuntu/xenial64"
    h.vm.network "private_network", ip: "192.168.135.121" 
    h.vm.provision :shell, inline: 'command -v python > /dev/null || apt-get -y install python-minimal'
    h.vm.hostname = "db01"
      h.vm.provision "ansible" do |ansible| 
      ansible.verbose = "v" 
      ansible.playbook = "mysqldb.yml" 
    end 
    
  end
end
