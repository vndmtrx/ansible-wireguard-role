# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |vps|
  vps.vm.box = "debian/bookworm64"
  vps.vm.hostname = "ansible-wireguard-role-vagrant"
  vps.vm.network "public_network",
    :use_dhcp_assigned_default_route => true,
    :adapter => 2,
    :mac => "080027123456"
  vps.vm.provider "virtualbox" do |v|
    v.memory = 2048
    v.cpus = 2
    v.default_nic_type = "virtio"
    v.customize ["modifyvm", :id, "--natnet1", "10.254.0.0/16"]
  end

  vps.vm.provision :"ansible" do |ansible|
    ansible.playbook = "ansible/playbook.yml"
    ansible.config_file = "ansible/.ansible.cfg"
  end
end
