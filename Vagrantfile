# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|
  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://atlas.hashicorp.com/search.
  config.vm.box = "ubuntu/xenial64"
  N = 2
  (1..N).each do |machine_id|
     config.vm.define "machine#{machine_id}" do |machine|
        machine.vm.hostname = "machine#{machine_id}"
        machine.vm.network "private_network", ip: "192.168.77.#{20+machine_id}"
        config.vm.provider "virtualbox" do |v|
         v.memory = 1024
        end       
        # Only execute the Ansible provisioner,
        # when all the machines are up and ready.
        if machine_id == N
           machine.vm.provision :ansible do |ansible|
              # Disable default limit to connect to all the machines
              ansible.limit = "all"
              ansible.playbook = "provisioning/playbook.yaml"
	      ansible.groups = {
	        "orchestrated"  => ["machine1"],
           "load_balancer" => ["machine2"]     
	      }
           end
        end
     end
  end
end
