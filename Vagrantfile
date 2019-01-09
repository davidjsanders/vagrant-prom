# -*- mode: ruby -*-
# vi: set ft=ruby :

prefix="sscisapp"

# Configure the VM
Vagrant.configure("2") do |config|
  config.vm.box = "centos/7"

  # config.vm.define "sscisapp9-vm", primary: true do |sscisapp9|
  #   sscisapp9.vm.network "forwarded_port", guest: 22, host: 2209
  #   sscisapp9.vm.network "forwarded_port", guest: 3000, host: 20901
  #   sscisapp9.vm.network "forwarded_port", guest: 9090, host: 20902
  #   sscisapp9.vm.network "forwarded_port", guest: 8080, host: 20903
  #   sscisapp9.vm.network "forwarded_port", guest: 9100, host: 20904
  #     sscisapp9.vm.hostname = "#{prefix}9"
  #   sscisapp9.vm.network "private_network", ip: "10.70.0.9"
  # end

  N=12
  (9..N).each do |machine_id|
    config.vm.define "sscisapp#{machine_id}-vm" do |node|
        if machine_id == 9
          node.vm.network "forwarded_port", guest: 22, host: "2209"
          node.vm.network "forwarded_port", guest: 3000, host: 23000
          node.vm.network "forwarded_port", guest: 9090, host: 29090
          node.vm.network "forwarded_port", guest: 8080, host: 28080
          node.vm.network "forwarded_port", guest: 9100, host: 29100
        else
          node.vm.network "forwarded_port", guest: 22, host: "22#{machine_id}"
        end
        node.vm.hostname = "#{prefix}#{machine_id}"
        node.vm.network "private_network", ip: "10.70.0.#{machine_id}"
    end
  end

  config.vm.provider :virtualbox do |vb|
    vb.cpus = 1
    vb.memory = "1024"
    vb.customize ["modifyvm", :id, "--audio", "none"]
  end

  config.vm.provision "file", source: "./config/hosts", destination: "/tmp/hosts"
  config.vm.provision :shell, inline: "cat /tmp/hosts | sudo tee -a /etc/hosts"
  config.vm.provision :shell, inline: "rm -f /tmp/dasander.pub"
  config.vm.provision "file", source: "~/.ssh/id_rsa.pub", destination: "/tmp/dasander.pub"
  config.vm.provision :shell, inline: "cat /tmp/dasander.pub | sudo tee -a /home/vagrant/.ssh/authorized_keys"
  config.vm.provision :shell, inline: "rm -f /tmp/dasander.pub"
end
