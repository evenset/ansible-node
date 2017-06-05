# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = "ubuntu/xenial64"


  config.vm.network "private_network", ip: "192.168.20.20"


  config.vm.provider "virtualbox" do |vb|
    vb.customize ["modifyvm", :id, "--name", "MyNodeApp", "--memory", "1024"]
  end

  # Shared folder from the host machine to the guest machine.
  # To enable it, uncomment the line below and fix the correct paths [refer to ansbile]
  #config.vm.synced_folder "../../../my-node-app", "/webapps/mynodeapp/my-node-app"

  # Ansible provisioner.
  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "vagrant.yml"
    ansible.host_key_checking = false
    ansible.verbose = "vv"
  end
end