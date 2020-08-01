# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://vagrantcloud.com/search.
  config.vm.box = "generic/ubuntu2004"
  config.vm.hostname = "endor"

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
  # config.vm.network "forwarded_port", guest: 80, host: 55031, host_ip: "127.0.0.1"
  config.vm.network "forwarded_port", guest: 22, host: 55031, host_ip: "127.0.0.1", auto_correct: true
  config.vm.network "forwarded_port", guest: 5901, host: 55032, host_ip: "127.0.0.1", auto_correct: true
  config.vm.network "forwarded_port", guest: 5902, host: 55033, host_ip: "127.0.0.1",  auto_correct: true
  config.vm.network "forwarded_port", guest: 3389, host: 55034, host_ip: "127.0.0.1",  auto_correct: true
  config.vm.network "forwarded_port", guest: 8000, host: 58000
  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  # config.vm.network "private_network", ip: "192.168.33.10"

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  # config.vm.network "public_network"
  config.vm.network "private_network", ip: "10.55.55.3"
  #config.vm.network "private_network", ip: "10.55.55.105"

  # https://www.vagrantup.com/docs/synced-folders/basic_usage.html
  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  # config.vm.synced_folder "../data", "/vagrant_data"
  config.vm.synced_folder	"../../bind",	"/bind", owner: "2001", group: "2001", create: true
  config.vm.synced_folder	"../../",	"/vagrant", owner: "2001", group: "2001"
  #config.vm.synced_folder "../../.keys", "/thedr/.ssh", owner: "2001", group: "2001", create: true, mount_options: ["uid=2001", "gid=2001"]
  #config.vm.synced_folder "../../thedr", "/thedr/", owner: "2001", group: "2001", create: true, mount_options: ["uid=2001", "gid=2001"]
  #config.vm.synced_folder "../../ansible", "/ansible", owner: "2001", group: "2001"
  #config.vm.synced_folder "../../blueprints", "/blueprints", owner: "2001", group: "2001", create: true
  #config.vm.synced_folder "../../ansible/etc/ansible/", "/etc/ansible/", owner: "2001", group: "2001"
  config.vm.synced_folder "../../repos", "/repos", owner: "2001", group: "2001", create: true
  config.vm.synced_folder "../../Downloads", "/Downloads", owner: "2001", group: "2001", create: true

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  #
  disk = 'extra_disk.vdi'
    config.vm.provider "virtualbox" do |vb|
      # Display the VirtualBox GUI when booting the machine
  #   vb.gui = true
   
      # Customize the amount of memory on the VM:
      vb.name = "Endor"
      vb.memory = "1024"
      vb.customize ["modifyvm", :id, "--cpuexecutioncap", "50"]
      #unless File.exist?(disk)
      #  vb.customize ['createhd', '--filename', disk, '--size', 40 * 1024]
      #end
      #vb.customize ['storageattach', :id, '--storagectl', 'IDE Controller', '--port', 1, '--device', 0, '--type', 'hdd', '--medium', disk]

      #vb.customize ['modifyvm', :id, '--nictype0', 'virtio']
      vb.customize ['modifyvm', :id, '--nictype1', 'virtio']
      #vb.customize ['modifyvm', :id, '--nictype2', 'virtio']
      #vb.customize ['modifyvm', :id, '--nictype3', 'virtio']


    end
  #
  # View the documentation for the provider you are using for more
  # information on available options.

  # Enable provisioning with a shell script. Additional provisioners such as
  # Ansible, Chef, Docker, Puppet and Salt are also available. Please see the
  # documentation for more information about their specific syntax and use.
   config.vm.provision "shell", inline: <<-SHELL
     apt-get update
     apt-get install -yq apache2 make docker docker.io net-tools ansible dos2unix supervisor # linux-headers--$(uname -r) build-essential dkms
     file /vagrant/functions/ready | grep CRLF && dos2unix -n /vagrant/functions/ready /usr/local/bin/ready
     file /vagrant/functions/ready | grep CRLF || cp /vagrant/functions/ready /usr/local/bin/ready
     chmod 0700 /usr/local/bin/ready
     ready

     # egrep "10.55.55.2	ns1.endore.local ns1" /etc/hosts 2>/dev/null || echo "10.55.55.2	ns1.endore.local ns1" >> /etc/hosts

     pull_repos
	 get_repo spicerack blueprints:galaxy3-net/spicerack.git master
	 get_repo named named:galaxy3-net/named.git master
     get_repo blueprints blueprints:galaxy3-net/blueprints.git master
     get_repo quarren quarren:galaxy3-net/quarren.git master
     #get_repo nakadia nakadia:galaxy3-net/nakadia.git master

     tenable named
     tenable quarren
     #setup_named
     setup_resolver
     #docker ps -a
     #setup_quarren

SHELL
end

