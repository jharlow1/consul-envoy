# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  config.vm.box = "centos/7"

  config.vm.define "frontend" do |client|
    client.vm.hostname = "frontend.envoydemo"
    client.vm.network "private_network", ip: "192.168.99.2"
  end

  config.vm.define "service1" do |server|
    server.vm.hostname = "service1.envoydemo"
    server.vm.network "private_network", ip: "192.168.99.3"
    server.vm.network "forwarded_port", guest: 19001, host: 19001
  end

  config.vm.define "service2" do |server|
    server.vm.hostname = "service2.envoydemo"
    server.vm.network "private_network", ip: "192.168.99.4"
  end

  config.vm.define "consul" do |server|
    server.vm.hostname = "consul.envoydemo"
    server.vm.network "private_network", ip: "192.168.99.5"
    server.vm.network "forwarded_port", guest: 8500, host: 8500
  end

  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "provisioning/main.yaml"
    ansible.become = true
    ansible.host_vars = {
      "frontend" => { "ip_address" => "192.168.99.2" },
      "service1" => { "ip_address" => "192.168.99.3", "service_name" => "Service 1" },
      "service2" => { "ip_address" => "192.168.99.4", "service_name" => "Service 2" },
      "consul"   => { "ip_address" => "192.168.99.5" }
    }
    ansible.groups = {
      "service" => [ "service1", "service2" ]
    }
  end
end
