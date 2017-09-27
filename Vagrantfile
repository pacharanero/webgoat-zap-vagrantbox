# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile for WebGoat and OWASP Zap pentesting/security learning environment

unless Vagrant.has_plugin?("vagrant-docker-compose")
  system("vagrant plugin install vagrant-docker-compose")
  puts "Dependencies installed, please try the command again."
  exit
end

Vagrant.configure("2") do |config|
  # this Vagrant box includes the Unity desktop environment (most Vagrant boxes are headless)
  config.vm.box = "boxcutter/ubuntu1604-desktop"
  config.vm.box_check_update = true

  ## PORT FORWARDING SETTINGS
  # the 'autocorrect' flag corrects any port clashes on your local machine
  # (but can introduce inconsistency about on which port a given service is located)
  # config.vm.network "forwarded_port", guest: 80, host: 80, host_ip: "127.0.0.1", auto_correct: true
  # config.vm.network "private_network", ip: "192.168.33.10"
  # config.vm.network "public_network"

  config.vm.synced_folder ".", "/vagrant"

  config.vm.provider "virtualbox" do |vb|
    # Display the VirtualBox GUI when booting the machine
    vb.gui = true
    # Switch off 3D Acceleration on the gues as it is very host-dependent
    vb.customize ["modifyvm", :id, "--accelerate3d", "off"]
    # Customize the amount of memory on the VM:
    # vb.memory = "3072"
  end

  ## PROVISIONING

  # OSupdates
  # config.vm.provision :shell, inline: "sudo apt-get update"
  # config.vm.provision :shell, inline: "sudo apt-get upgrade -y"
  # config.vm.provision :shell, inline: "sudo apt-get -y autoremove"

  # java SDK
  config.vm.provision :shell, inline: "sudo apt-get -y install openjdk-8-jdk"

  # install docker
  config.vm.provision :docker

  # install docker-compose (runs several containers and links them together automatically)
  config.vm.provision :docker_compose, yml: "/vagrant/docker-compose.yml", rebuild: true, run: "always"

  unless File.exist? File.expand_path "/vagrant/ZAP_2_6_0_unix.sh"
    config.vm.provision :shell, inline: "wget --progress=bar:force https://github.com/zaproxy/zaproxy/releases/download/2.6.0/ZAP_2_6_0_unix.sh"
    config.vm.provision :shell, inline: "chmod +x ZAP_2_6_0_unix.sh"
  end

end
