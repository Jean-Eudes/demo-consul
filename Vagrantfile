# -*- mode: ruby -*-
# vi: set ft=ruby :
$script = <<SCRIPT

echo Installing dependencies...
sudo apt-get update
sudo apt-get install -y unzip curl

echo Fetching Consul...
cd /tmp/
wget https://releases.hashicorp.com/consul/0.6.1/consul_0.6.1_linux_amd64.zip -O consul.zip
wget https://releases.hashicorp.com/consul-template/0.14.0/consul-template_0.14.0_linux_amd64.zip -O consul-template.zip

echo Installing Consul...
unzip consul.zip
unzip consul-template.zip

sudo chmod +x consul
sudo chmod +x consul-template

sudo mv consul /usr/bin/consul
sudo mv consul-template /usr/bin/consul-template

sudo mkdir /etc/consul.d
sudo chmod a+w /etc/consul.d

SCRIPT

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = "debian/wheezy64"

  config.vm.provision "shell", inline: $script

  config.vm.define "server" do |n1|
      n1.vm.hostname = "server"
      n1.vm.network "private_network", ip: "172.20.20.10"
      n1.vm.provision "shell", inline: <<-SHELL
        cp /vagrant/history_server /home/vagrant/.bash_history
      SHELL
  end

  config.vm.define "n1" do |n1|
      n1.vm.hostname = "web1"
      n1.vm.network "private_network", ip: "172.20.20.20"
      n1.vm.provision "shell", inline: <<-SHELL
        cp /vagrant/history_web1 /home/vagrant/.bash_history
        echo "deb http://ftp.debian.org/debian wheezy-backports main" >> /etc/apt/sources.list
        apt-get update && apt-get install jq
      SHELL
  end

  config.vm.define "n2" do |n2|
      n2.vm.hostname = "web2"
      n2.vm.network "private_network", ip: "172.20.20.21"
      n2.vm.provision "shell", inline: <<-SHELL
        cp /vagrant/history_web2 /home/vagrant/.bash_history
      SHELL
  end

  config.vm.define "proxy" do |n1|
      n1.vm.hostname = "balancer"
      n1.vm.network "private_network", ip: "172.20.20.30"
      n1.vm.provision "shell", inline: <<-SHELL
        cp /vagrant/history_balancer /home/vagrant/.bash_history
      SHELL
  end
end
