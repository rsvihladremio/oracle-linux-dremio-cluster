# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "oraclelinux/8"

  config.vm.box_url = "https://oracle.github.io/vagrant-projects/boxes/oraclelinux/8.json"

  config.vm.define "coordinator" do |coordinator|
    coordinator.vm.network "private_network", ip: "192.168.56.10"
    coordinator.vm.provision "shell", inline: <<-SHELL
        sed -i 's/executor.enabled: true/executor.enabled: false/g' /etc/dremio.conf
    SHELL
  end

  config.vm.define "executor1" do |executor1|
    executor1.vm.network "private_network", ip: "192.168.56.11"
    executor1.vm.provision "file", source: "./ee_linux_dremio-enterprise-21.2.0-202205262146080444_038d6d1b_1.noarch.rpm", destination:  "/home/vagrant/ee_linux_dremio-enterprise-21.2.0-202205262146080444_038d6d1b_1.noarch.rpm"
    executor1.vm.provision "shell", inline: <<-SHELL
        sed -i 's/coordinator.enabled: true/coordinator.enabled: false/g' /etc/dremio.conf
        sed -i 's/coordinator.master.enabled: true/coordinator.master.enabled: false/g' /etc/dremio.conf
    SHELL
  end

  config.vm.define "executor2" do |executor2|
    executor2.vm.network "private_network", ip: "192.168.56.12"
    executor2.vm.provision "shell", inline: <<-SHELL
        sed -i 's/coordinator.enabled: true/coordinator.enabled: false/g' /etc/dremio.conf
        sed -i 's/coordinator.master.enabled: true/coordinator.master.enabled: false/g' /etc/dremio.conf
    SHELL
  end

  config.vm.provider "virtualbox" do |vb|
     vb.memory = "4096"
     vb.cpus = "2"
  end
  
  config.vm.provision "file", source: "./ee_linux_dremio-enterprise-21.2.0-202205262146080444_038d6d1b_1.noarch.rpm", destination:  "/home/vagrant/ee_linux_dremio-enterprise-21.2.0-202205262146080444_038d6d1b_1.noarch.rpm"
  config.vm.provision "shell", inline: <<-SHELL
        yum update -y
        yum install java-1.8.0-openjdk -y  
        yum install /home/vagrant/ee_linux_dremio-enterprise-21.2.0-202205262146080444_038d6d1b_1.noarch.rpm -y
        chkconfig --level 3456 dremio on
        echo "zookeeper \"192.168.56.10:2181\"" >> /etc/dremio.conf
   SHELL
end
