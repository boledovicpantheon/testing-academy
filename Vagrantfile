# -*- mode: ruby -*-
# vi: set ft=ruby :

IMAGE_NAME = "boledovicpantheon/testing-academy-22.04_LTS" # https://app.vagrantup.com/boledovicpantheon/boxes/testing-academy-22.04_LTS
IMAGE_VERSION = "0.1.0" 

WORKER_COUNT = 2   # number of worker nodes
WORKER_RAM = 2048
WORKER_CPUS = 2

MASTER_COUNT = 1   # number of master nodes
MASTER_RAM = 8192
MASTER_CPUS = 4

Vagrant.configure("2") do |config|

# we will use this to pass the join command from master to the nodes
  config.vm.synced_folder ".", "/vagrant", disabled: false   

# disable automatic box update checking
  config.vbguest.no_install = true
  config.vbguest.auto_update = false 
  config.vbguest.no_remote = true
  config.vbguest.auto_reboot = true

###  Worker node(s)
(1..WORKER_COUNT).each do |i|
  config.vm.define "node-#{i}" do |node|
    node.vm.box = "#{IMAGE_NAME}"
    node.vm.box_version = "#{IMAGE_VERSION}"
    node.vm.network "private_network", ip: "192.168.56.#{100 + i}"
    node.vm.hostname = "node-#{i}"
    node.vm.network "forwarded_port", guest: 22, host: "#{20300 + i}"

    node.vm.provider "virtualbox" do |v| 
      v.memory = "#{WORKER_RAM}"
      v.cpus = "#{WORKER_CPUS}"
      v.name = "node-#{i}"
    end
  end
end
  
###  Master node(s)
  (1..MASTER_COUNT).each do |i|
    config.vm.define "master-#{i}" do |master|
      master.vm.box = "#{IMAGE_NAME}"
      master.vm.box_version = "#{IMAGE_VERSION}"
      master.vm.network "private_network", ip: "192.168.56.#{10 + i}"
      master.vm.hostname = "master-#{i}"
      master.vm.network "forwarded_port", guest: 22, host: "#{20200 + i}"
    
      master.vm.provider "virtualbox" do |v| 
        v.memory = "#{MASTER_RAM}"
        v.cpus = "#{MASTER_CPUS}"
        v.name = "master-#{i}"
      end

      master.vm.provision "ansible_local" do |ansible|
        ansible.provisioning_path = "/vagrant"
        ansible.playbook = "ansible/site.yaml"
        ansible.verbose = "true" # same as `-v`
        ansible.become = "true" #sudo
        ansible.limit = "all"
        ansible.inventory_path = "ansible/inventory/"
        ansible.config_file = "ansible/ansible.cfg"
        ansible.raw_arguments = ["--vault-pass-file=/vagrant/ansible/vault/secret"]
      end
    end
  end

end




