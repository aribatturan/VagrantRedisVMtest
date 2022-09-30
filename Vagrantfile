# -*- mode: ruby -*-
# vi: set ft=ruby :


cluster = {
  "master" => { :ip => "192.168.33.10", :cpus => 2, :mem => 4096 },
  "slave-0" => { :ip => "192.168.33.11", :cpus => 2, :mem => 2048 },
  "slave-1" => { :ip => "192.168.33.12", :cpus => 2, :mem => 2048 }
}

Vagrant.configure("2") do |config|
  config.vm.box = "geerlingguy/ubuntu2004"

  cluster.each_with_index do |(hostname, info), index|
    config.vm.define hostname do |cfg|
      cfg.vm.network :private_network, ip: "#{info[:ip]}"
      cfg.vm.hostname = hostname
      cfg.vm.provider "virtualbox" do |vb|
        # Display the VirtualBox GUI when booting the machine
        vb.gui = false
      
        # Customize the amount of memory on the VM:
        vb.memory = info[:mem]
        vb.cpus = info[:cpus]
      end

      cfg.vm.provision "shell", inline: "/vagrant/redis-installer.sh"

      if hostname == "master"
        cfg.vm.provision "shell", inline: "/vagrant/redis-master.sh", privileged: false
      end

      if hostname == "slave-0"
        cfg.vm.provision "shell", inline: "/vagrant/redis-slave-0.sh", privileged: false
      end

      if hostname == "slave-1"
        cfg.vm.provision "shell", inline: "/vagrant/redis-slave-1.sh", privileged: false
      end
    end
  end
end