# -*- mode: ruby -*-
# vi: set ft=ruby :

BOX_IMAGE = "centos/7"
NODE_COUNT = 4

Vagrant.configure("2") do |config|
  (1..NODE_COUNT).each do |i|
    config.vm.define "node#{i}" do |subconfig|
      subconfig.vm.box = BOX_IMAGE
      subconfig.dns.tld = "node#{i}"
      subconfig.vm.provider :virtualbox do |vb|
         vb.memory = 1024
         vb.cpus = 2
         vb.gui = false
      end
    
      subconfig.vm.provision :ansible, playbook: "/vagrant/inicio.yml"

      if i == 1 
        subconfig.vm.hostname = "master"
        subconfig.vm.network :private_network, ip: "10.0.0.10"
        subconfig.vm.provision :ansible, playbook: "/vagrant/master.yml" 
      else
        subconfig.vm.hostname = "node#{i}"
        subconfig.vm.network :private_network, ip: "10.0.0.#{i + 20}"
        subconfig.vm.provision :ansible, playbook: "/vagrant/nodes.yml"
      end	
    end
  end
  if Vagrant.has_plugin?("vagrant-cachier")
    config.cache.scope = :box
  end
end

