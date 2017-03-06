# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|
  if (/cygwin|mswin|mingw|bccwin|wince|emx/ =~ RUBY_PLATFORM) != nil
    config.vm.synced_folder ".", "/vagrant", mount_options: ["dmode=700,fmode=600"]
  else
    config.vm.synced_folder ".", "/vagrant"
  end 
  config.vm.define "vip-linux-sqlserver-01" do |d|
    d.vm.box ="ubuntu/trusty64"
    d.vm.hostname = "vip-linux-sqlserver-01"
    d.vm.network "public_network", bridge: "eno4", ip: "192.168.57.79", auto_config: "false", netmask: "255.255.255.0" , gateway: "192.168.57.1"
    default_router = "192.168.57.1"
    d.vm.provision :shell, inline: "ip route delete default 2>&1 >/dev/null || true; ip route add default via #{default_router}" 
    d.vm.provision :shell, path: "scripts/bootstrap4Ubuntu_ansible.sh"
    d.vm.provision :shell, inline: "PYTHONUNBUFFERED=1 ansible-playbook /vagrant/ansible/mssql.yml -c local"
    d.vm.provider "virtualbox" do |v|
      v.memory = 8192
      v.cpus = 2
    end
  end
  config.vm.define "vip-linux-sqlserver-02" do |d|
    d.vm.box ="ubuntu/trusty64"
    d.vm.hostname = "vip-linux-sqlserver-02"
    d.vm.network "public_network", bridge: "eno4", ip: "192.168.57.80", auto_config: "false", netmask: "255.255.255.0" , gateway: "192.168.57.1"
    default_router = "192.168.57.1"
    d.vm.provision :shell, inline: "ip route delete default 2>&1 >/dev/null || true; ip route add default via #{default_router}" 
    d.vm.provision :shell, path: "scripts/bootstrap4Ubuntu_ansible.sh"
    d.vm.provision :shell, inline: "PYTHONUNBUFFERED=1 ansible-playbook /vagrant/ansible/mssql.yml -c local"
    d.vm.provider "virtualbox" do |v|
      v.memory = 8192
      v.cpus = 2
    end
  end
  if Vagrant.has_plugin?("vagrant-cachier")
    config.cache.scope = :box
  end
end
