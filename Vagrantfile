# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|

######  config.vm.box = "bento/centos-7.5"
  config.vm.box = "bento/ubuntu-16.04"

  config.vm.network "public_network", ip: "192.168.0.104", bridge: "eth1"

  config.vm.provider "virtualbox" do |v|
      v.name = "SA-project Ubuntu"
      v.memory = 2048
      v.cpus   = 2
      v.gui = false
  end

  config.vm.provision "kubectl", type: "shell",  inline: <<-SCRIPT
	echo "Installing pip"
	sudo apt update
	sudo apt install python-pip sshpass ansible -y
	
	sudo pip install virtualenv --upgrade
	sudo pip install -U setuptools
	sudo pip install "ansible==2.8"

	echo "Cloning sa-project"
	su - vagrant
	git clone https://github.com/begun74/sa-project.git
	chown -R vagrant.vagrant ./sa-project

	echo "Ssh-keygen & ssh-copy-id )"
	cat /dev/zero |ssh-keygen -t rsa -b 4096  -N ""
	sshpass -p vagrant ssh-copy-id -o StrictHostKeyChecking=no vagrant@192.168.0.104

	echo "Ansible install xwiki project"
	cd sa-project
	sudo ansible-playbook xwiki.yaml -i inventory/main.yaml
  SCRIPT

end