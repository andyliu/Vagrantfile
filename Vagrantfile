# -*- mode: ruby -*-
# vi: set ft=ruby :

$script = <<SCRIPT
    echo "start inline script here"
	mkdir andy_inline_test
SCRIPT

$script_test = <<SCRIPT
    echo "start inline script here"
	mkdir andy_inline_test
SCRIPT

app_servers = {
    :http => '192.168.58.20',
    :php => '192.168.58.21'
}

Vagrant.configure("2") do |config|
   # config.vm.box = "u1404"
 
    config.vm.define :andy do |andy_config|
		andy_config.vm.box = "hashicorp/precise64"
        andy_config.vm.network :private_network, ip: "192.168.58.10"
        andy_config.vm.network :forwarded_port, guest: 80, host: 8080
        andy_config.vm.provider :virtualbox do |vb|
            vb.name = "andyvb"
            vb.customize ["modifyvm", :id, "--memory", "256"]
        end
		andy_config.vm.provision "shell", path: "andy.sh"
		andy_config.vm.provision "shell", inline: $script_test, privileged: false
    end
 
    app_servers.each do |app_server_name, app_server_ip|
        config.vm.define app_server_name do |app_config|
            app_config.vm.hostname = "#{app_server_name.to_s}.vagrant.internal"
            app_config.vm.network :private_network, ip: app_server_ip
           # app_config.vm.synced_folder "../app", "/opt/app"
            app_config.vm.provider "virtualbox" do |vb|
                vb.name = app_server_name.to_s
                vb.customize ["modifyvm", :id, "--memory", "256"]
            end
        end
    end
 
    config.vm.define :redis do |redis_config|
		redis_config.vm.box = "u1404"
        redis_config.vm.hostname = "redis.vagrant.internal"
        redis_config.vm.network :private_network, ip: "192.168.58.30"
        redis_config.vm.provider "virtualbox" do |vb|
            vb.name = "redis"
            vb.customize ["modifyvm", :id, "--cpuexecutioncap", "50"]
            vb.customize ["modifyvm", :id, "--memory", "256"]
        end
		redis_config.vm.provision "shell", inline: $script, privileged: false
		redis_config.vm.provision "shell", path: "andy.sh"
    end
end