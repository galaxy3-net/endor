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
  # config.vm.network "forwarded_port", guest: 80, host: 55031, host_ip: "127.0.0.1",  auto_correct: true
  #config.vm.network "forwarded_port", guest: 22, host: 55031, host_ip: "127.0.0.1", auto_correct: true
  #config.vm.network "forwarded_port", guest: 5901, host: 55032, host_ip: "127.0.0.1", auto_correct: true
  #config.vm.network "forwarded_port", guest: 5902, host: 55033, host_ip: "127.0.0.1",  auto_correct: true
  config.vm.network "forwarded_port", guest: 3389, host: 3389,  auto_correct: true
  config.vm.network "forwarded_port", guest: 8000, host: 8000,  auto_correct: true
  config.vm.network "forwarded_port", guest: 80, host: 80, auto_correct: true
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
  config.vm.synced_folder "../../repos", "/repos", owner: "2001", group: "2001", create: true
  config.vm.synced_folder "../../Downloads", "/Downloads", owner: "2001", group: "2001", create: true
  config.vm.synced_folder "../../.g3", "/Config", owner: "2001", group: "2001", create: true

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  #
  disk = 'extra_disk.vdi'
    config.vm.provider "virtualbox" do |vb|
      # Customize the amount of memory on the VM:
      vb.name = "Endor (Core)"
      vb.gui = false
      vb.memory = "1024"
      vb.customize ["modifyvm", :id, "--cpuexecutioncap", "50"]
      vb.customize ['modifyvm', :id, '--nicpromisc1', 'allow-all']
      vb.customize ['modifyvm', :id, '--nictype1', 'virtio']
      vb.customize ['modifyvm', :id, '--nicpromisc1', 'allow-all']
      vb.customize ['modifyvm', :id, '--nictype2', 'virtio']
      vb.customize ['modifyvm', :id, '--nicpromisc2', 'allow-all']
    end
  #
  # View the documentation for the provider you are using for more
  # information on available options.

  # Enable provisioning with a shell script. Additional provisioners such as
  # Ansible, Chef, Docker, Puppet and Salt are also available. Please see the
  # documentation for more information about their specific syntax and use.
   config.vm.provision "shell", inline: <<-SHELL
     tr -d '\r' < /vagrant/functions/ready >/usr/local/bin/ready && chmod 0700 /usr/local/bin/ready
     /usr/local/bin/ready
     apt-get install -y ansible
     ansible-galaxy install -r /vagrant/requirements.yml
     # /usr/local/bin/install_pkgs | tee -a /var/log/install_pkgs.log 2>&1

     #/usr/local/bin/g3enable named
     #/usr/local/bin/g3enable quarren
     #setup_resolver
     #setup_xrdp
     #setup_vnc
SHELL
  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "playbook.yml"
  end
end
