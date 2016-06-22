# -*- mode: ruby -*-
# vi: set ft=ruby :


Vagrant.configure(2) do |config|
 	config.vm.box = "larryli/wily64"
 	
	config.vm.provider "virtualbox" do |v|
 		v.memory = 2048
 	  v.cpus = 2
	end
	
  config.vm.define "swarmmgr1" do |config|
		config.vm.hostname = "swarmmgr1"
	  config.vm.network "private_network", ip: "192.168.2.10"
	end

	config.vm.define "swarmnode1" do |config|
		config.vm.hostname = "swarmnode1"
	  config.vm.network "private_network", ip: "192.168.2.11"
	end

	config.vm.define "swarmnode2" do |config|
		config.vm.hostname = "swarmnode2"
	  config.vm.network "private_network", ip: "192.168.2.12"
	end
	
	config.vm.define "swarmnode3" do |config|
		config.vm.hostname = "swarmnode3"
	  config.vm.network "private_network", ip: "192.168.2.13"
	end

end
