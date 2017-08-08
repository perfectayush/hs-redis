# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure(2) do |config|
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://atlas.hashicorp.com/search.


  if Vagrant.has_plugin?("nugrant")
    config.user.defaults = {
      'proxy' => {
        'no_proxy' => "localhost,127.0.0.1,.example.com,.vagrant.vm",
        'enabled' => false,
        'endpoint' => false
      }
    }

    if Vagrant.has_plugin?("vagrant-proxyconf")
      config.proxy.enabled = config.user.proxy.enabled
      config.proxy.http = config.user.proxy.endpoint
      config.proxy.https = config.user.proxy.endpoint
      config.apt_proxy.http = config.user.proxy.endpoint
      config.apt_proxy.https = config.user.proxy.endpoint
      config.proxy.no_proxy = config.user.proxy.no_proxy
    end
  end

  config.vm.box = "ubuntu/trusty64"


  config.vm.provider "virtualbox" do |vb|
    vb.customize ["modifyvm", :id, "--memory", "1024"]
    vb.customize ["modifyvm", :id, "--natdnshostresolver1", "off"]
  end

  config.vm.define "redis1" do |redis1|
    redis1.vm.hostname = "redis1"
    redis1.vm.network "private_network", ip: "172.20.29.10"
  end

  config.vm.define "redis2" do |redis2|
    redis2.vm.hostname = "redis2"
    redis2.vm.network "private_network", ip: "172.20.29.11"
  end

  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "redis_cluster.yml"
    ansible.limit = "all"
    ansible.groups = {
      "cl_redis-vagrant" => ["redis1","redis2"],
      "tag_Name_redis1" => ["redis1"],
      "tag_Name_redis2" => ["redis2"]
    }

    ansible.host_vars = {
      "redis1" => {"ec2_private_ip_address" => "172.20.29.10", "ec2_tag_Name" => "redis1"},
      "redis2" => {"ec2_private_ip_address" => "172.20.29.11", "ec2_tag_Name" => "redis2"}
    }
  end

end
