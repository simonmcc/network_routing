# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure(2) do |config|
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://atlas.hashicorp.com/search.
  config.vm.box = "ubuntu/trusty64"

  # Disable automatic box update checking. If you disable this, then
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  # config.vm.box_check_update = false

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  config.vm.network "private_network", ip: "192.168.33.10"

  # Make sure VLAN tagging works on VirtualBox VMs
  config.vm.provider "virtualbox" do |vb|
    # Set the private network nic to be AMD PCI, not Intel, so that VLAN
    # tagging works (eth1, eth0 is reserved for Vagrant work, don't tweak)
    # VBoxManage modifyvm $box --nictype1 virtio
#    vb.customize = ["modifyvm", :id, "--nictype2", "virtio"]

    # Customize the amount of memory on the VM:
    vb.memory = "1024"
  end

  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "vagrant-playbook.yml"
  end
end
