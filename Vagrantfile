# -*- mode: ruby -*-
# vi: set ft=ruby :


IMAGE_NAME = "bento/ubuntu-16.04"
N = ENV['NUM_NODES'] || 2

Vagrant.configure("2") do |config|
  config.ssh.insert_key = false
  config.vm.provider "virtualbox" do |v|
      v.memory = 3072
      v.cpus = 2
  end
    
  config.vm.define "k8s-master" do |master|
      master.vm.box = IMAGE_NAME
      master.vm.network "private_network", ip: "192.250.50.10"
      master.vm.hostname = "k8s-master"
      master.vm.provision "ansible" do |ansible|
          ansible.playbook = "kubernetes-setup/master-playbook.yml"
          ansible.extra_vars = {
              node_ip: "192.250.50.10",
          }
      end
  end

  (1..N).each do |i|
      config.vm.define "k8s-node-#{i}" do |node|
          node.vm.box = IMAGE_NAME
          node.vm.network "private_network", ip: "192.250.50.#{i + 10}"
          node.vm.hostname = "k8s-node-#{i}"
          node.vm.provision "ansible" do |ansible|
              ansible.playbook = "kubernetes-setup/node-playbook.yml"
              ansible.extra_vars = {
                  node_ip: "192.250.50.#{i + 10}",
              }
          end

          # Only execute once the Ansible provisioner,
          # when all the machines are up and ready.
          #if i == N
          #  machine.vm.provision :ansible do |ansible|
          #      # Disable default limit to connect to all the machines
          #      ansible.limit = "all"
          #      ansible.playbook = "devops-tools/playbook.yml"
          #      ansible.groups = {
          #        "app_servers" => ["k8s-master"],
          #      }
          #    end
          #  end
      end
  end
end
