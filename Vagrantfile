# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "generic/ubuntu2004"
  config.vm.hostname = "endor"
  config.vm.network "forwarded_port", guest: 3389, host: 3389,  auto_correct: true
  config.vm.network "forwarded_port", guest: 8000, host: 8000,  auto_correct: true
  config.vm.network "forwarded_port", guest: 80, host: 80, auto_correct: true
  config.vm.network "private_network", ip: "10.55.55.3"
  config.vm.synced_folder	"../../bind",	"/bind", owner: "2001", group: "2001", create: true
  #config.vm.synced_folder	"../../",	"/vagrant", owner: "2001", group: "2001"
  config.vm.synced_folder "../../repos", "/repos", owner: "2001", group: "2001", create: true
  config.vm.synced_folder "../../Downloads", "/Downloads", owner: "2001", group: "2001", create: true
  config.vm.synced_folder "../../.g3", "/Config", owner: "2001", group: "2001", create: true

  config.vm.provision "file", source: "requirements.yml", destination: "requirements.yml"
  config.vm.provision "file", source: "playbook.yml", destination: "playbook.yml"
  config.vm.provision "file", source: "endor-role/", destination: "endor-role/"
  config.vm.provision "file", source: "../../functions", destination: "functions/bin"

  disk = 'extra_disk.vdi'
    config.vm.provider "virtualbox" do |vb|
      # Customize the amount of memory on the VM:
      vb.name = "Endor (Core)"
      # vb.gui = false
      vb.memory = "1024"
      vb.customize ["modifyvm", :id, "--cpuexecutioncap", "50"]
      vb.customize ['modifyvm', :id, '--nicpromisc1', 'allow-all']
      vb.customize ['modifyvm', :id, '--nictype1', 'virtio']
      vb.customize ['modifyvm', :id, '--nicpromisc1', 'allow-all']
      vb.customize ['modifyvm', :id, '--nictype2', 'virtio']
      vb.customize ['modifyvm', :id, '--nicpromisc2', 'allow-all']
  end

#  config.vm.provision "shell", inline: <<-SHELL
     #tr -d '\r' < /vagrant/functions/ready >/usr/local/bin/ready && chmod 0700 /usr/local/bin/ready
     #/usr/local/bin/ready
     #apt-get install -y ansible
     #ansible-galaxy install -r requirements.yml
#SHELL
  config.vm.provision "ansible_local" do |ansible|
    ansible.playbook = "/home/vagrant/playbook.yml"
    ansible.galaxy_role_file = "/home/vagrant/requirements.yml"
  end
end
