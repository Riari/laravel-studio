# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|
    config.vm.box = "blinkreaction/boot2docker"

    # Network config & shared folders
    config.vm.network "private_network", ip: "192.168.33.21"
    config.vm.synced_folder "./code", "/home/docker/laravel-studio", id: "laravel-studio-dev" , type: "smb"
    config.vm.synced_folder "~/.composer/", "/home/docker/.composer", id: "laravel-studio-composer" , type: "smb"

    # VM definition
    config.vm.provider "virtualbox" do |vb|
        vb.name = "laravel-studio"
        vb.memory = 1024
        vb.cpus = 1
    end

    # Bring up containers
    config.vm.provision "shell", run: "always", inline: "cd /home/docker/laravel-studio/phpdocker && docker-compose up -d 1>&2"

    # Redirect webserver port down 80, etc
    config.vm.provision "shell", run: "always", inline: "/usr/local/sbin/iptables -i eth1 -t nat -A PREROUTING -p tcp --dport 80 -j REDIRECT --to-port 9100 1>&2"

    # Disable guest additions auto update as it won't work on boot2docker, and slows vm boot down boot
    if Vagrant.has_plugin?("vagrant-vbguest")
        config.vbguest.auto_update = false
    end
end
