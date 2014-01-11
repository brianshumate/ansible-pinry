# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile for bootstrapping a Pinry instance on Mac OS X with Vagrant
# provider and Ansible provisioiner on Ubuntu virtual machine

VAGRANTFILE_API_VERSION = "2"
BOX_MEM = ENV['BOX_MEM'] || "1536"
BOX_NAME =  ENV['BOX_NAME'] || "ringtail64"
BOX_URI = ENV['BOX_URI'] || "http://opscode-vm-bento.s3.amazonaws.com/vagrant/virtualbox/opscode_ubuntu-13.04_chef-provisionerless.box"
CLUSTER_HOSTS = ENV['CLUSTER_HOSTS'] || "stage"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.define :pinry do |pinry_config|
    pinry_config.vm.box = BOX_NAME
    pinry_config.vm.box_url = BOX_URI
    pinry_config.vm.network :private_network, ip: "10.1.10.42"
    pinry_config.vm.hostname = "pinry.local"
    pinry_config.ssh.forward_agent = true
    pinry_config.vm.provider "virtualbox" do |v|
      v.name = "pinry"
      v.customize ["modifyvm", :id, "--memory", BOX_MEM]
      v.customize ["modifyvm", :id, "--ioapic", "on"]
      v.customize ["modifyvm", :id, "--cpus", "1"]
      v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
      v.customize ["modifyvm", :id, "--natdnsproxy1", "on"]
    end
    pinry_config.vm.provision :ansible do |ansible|
      ansible.inventory_path = CLUSTER_HOSTS
      ansible.playbook = "site.yml"
    end
  end
end

