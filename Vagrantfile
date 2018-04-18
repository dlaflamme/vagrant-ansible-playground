# -*- mode: ruby -*-
# vi: set ft=ruby :
BOX_IMAGE = "centos/7"
GUI = false
CPU = 1
RAM = 512

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|

  #
  # server
    config.vm.define "server.local" do |subconfig|
    server_ip = "10.90.0.10"

    subconfig.vm.box = BOX_IMAGE
    subconfig.vm.provider "virtualbox" do |vbox|
        vbox.gui    = GUI
        vbox.cpus   = CPU
        vbox.memory = RAM
    end

    subconfig.vm.hostname="server.local"
    subconfig.vm.network :private_network, ip: server_ip
    subconfig.vm.network "forwarded_port", guest: 80, host:10081

    subconfig.vm.provision "ansible_local" do |ansible|
      ansible.provisioning_path="/vagrant/ansible"
      ansible.inventory_path="/vagrant/ansible/environments/development/hosts"
      ansible.config_file="/vagrant/ansible/ansible.cfg"
      ansible.verbose = "vvvv"
      ansible.playbook = "playbook.yml"
    end
  end


  # this causes problems on windows if you have an older version
  # of Vagrant (< 2.0.1?)
  # see https://github.com/hashicorp/vagrant/issues/8954
  config.vm.synced_folder '.', '/vagrant', disabled: false

  # deal with box updates manually
  config.vm.box_check_update = false

  # keep guest additions plugin up-to-date
  config.vbguest.auto_update = true

  config.vm.provision "shell", inline: <<-SHELL
    yum --assumeyes update
    yum --assumeyes install epel-release
    yum --assumeyes install ansible
  SHELL



  # Defaults/examples from vagrant init below

  #
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://vagrantcloud.com/search.
  #config.vm.box = "debian/jessie64"

  # Disable automatic box update checking. If you disable this, then
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  # config.vm.box_check_update = false

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # NOTE: This will enable public access to the opened port
  # config.vm.network "forwarded_port", guest: 80, host: 8080

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine and only allow access
  # via 127.0.0.1 to disable public access
  # config.vm.network "forwarded_port", guest: 80, host: 8080, host_ip: "127.0.0.1"

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  # config.vm.network "private_network", ip: "192.168.33.10"

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  # config.vm.network "public_network"

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  # config.vm.synced_folder "../data", "/vagrant_data"

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  #
  # config.vm.provider "virtualbox" do |vb|
  #   # Display the VirtualBox GUI when booting the machine
  #   vb.gui = true
  #
  #   # Customize the amount of memory on the VM:
  #   vb.memory = "1024"
  # end
  #
  # View the documentation for the provider you are using for more
  # information on available options.

  # Enable provisioning with a shell script. Additional provisioners such as
  # Puppet, Chef, Ansible, Salt, and Docker are also available. Please see the
  # documentation for more information about their specific syntax and use.
  # config.vm.provision "shell", inline: <<-SHELL
  #   apt-get update
  #   apt-get install -y apache2
  # SHELL
end
