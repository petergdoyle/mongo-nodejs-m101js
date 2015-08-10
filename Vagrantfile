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
  config.vm.box = "CentOS-7-x86_64-MIN"

  #allows an ssh connection to be forwarded through the host machine on 2222
  #config.vm.network "forwarded_port", guest: 22, host: 2222, host_ip: "0.0.0.0", id: "ssh", auto_correct: true
  #allows mongo connection to be forwarded through the host machine on 227017
  #config.vm.network "forwarded_port", guest: 27017, host: 227017, host_ip: "0.0.0.0", id: "mongodb", auto_correct: true

  # Disable automatic box update checking. If you disable this, then
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  # config.vm.box_check_update = false

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # config.vm.network "forwarded_port", guest: 80, host: 8080

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  # config.vm.network "private_network", ip: "192.168.33.10"

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  # config.vm.network "public_network"

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  # config.vm.synced_folder "../data", "/vagrant_data"

  config.vm.synced_folder "mongo/", "/mongo"

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  #
  # config.vm.provider "virtualbox" do |vb|
  #   # Display the VirtualBox GUI when booting the machine
  #   vb.gui = true
  #
  #   # Customize the amount of memory on the VM:
  #   vb.memory = "1024"
  # end
  #
  # View the documentation for the provider you are using for more
  # information on available options.

  config.vm.provider "virtualbox" do |vb|
    vb.customize ["modifyvm", :id, "--cpuexecutioncap", "80"]
    vb.cpus=2 #recommended=4 if available
    vb.memory = "1024" #recommended=3072 or 4096 if available
  end

  # Define a Vagrant Push strategy for pushing to Atlas. Other push strategies
  # such as FTP and Heroku are also available. See the documentation at
  # https://docs.vagrantup.com/v2/push/atlas.html for more information.
  # config.push.define "atlas" do |push|
  #   push.app = "YOUR_ATLAS_USERNAME/YOUR_APPLICATION_NAME"
  # end

  # Enable provisioning with a shell script. Additional provisioners such as
  # Puppet, Chef, Ansible, Salt, and Docker are also available. Please see the
  # documentation for more information about their specific syntax and use.
  # config.vm.provision "shell", inline: <<-SHELL
  #   sudo apt-get update
  #   sudo apt-get install -y apache2
  # SHELL

  config.vm.provision "shell", inline: <<-SHELL

  #TODO if you need to get through a proxy for yum and other utilities...
  #export http_proxy=http://host.com:port/
  #export https_proxy=http://host.com:port/
  #export HTTP_PROXY=http://host.com:port/
  #export HTTPS_PROXY=http://host.com:port/
  #sudo echo "proxy=http://host.com:port/" >> /etc/yum.conf
  #sed -i "/# System wide functions and aliases/a export HTTP_PROXY=$HTTP_PROXY\\nexport HTTPS_PROXY=$HTTP_PROXY\\nexport http_proxy=$HTTP_PROXY\\nexport https_proxy=$HTTP_PROXY" /etc/bashrc

  #install node.js and npm
  yum -y install epel-release gcc gcc-c++
  yum -y install nodejs npm
  #TODO npm proxy settings if applicable
  #npm config set proxy $HTTP_PROXY
  #npm config set https-proxy $HTTP_PROXY
  npm install express -g
  npm install express-generator -g
  npm install forever -g
  #nice little utility to format a stream of json https://www.npmjs.com/package/format-json-stream
  npm install format-json-stream -g

  #install mongodb and start it and enable it at startup
  curl --insecure https://gist.githubusercontent.com/petergdoyle/7451a7f694b20df709cc/raw/b01b001478b40fc52f333b0ff9f9cb7ac2a25ac7/mongodb.repo -o mongodb.repo
  mv mongodb.repo /etc/yum.repos.d/
  yum -y install mongodb-org mongodb-org-server
  #sed -i 's@dbpath=/var/lib/mongo@dbpath=/vagrant/mongo/data@g' /etc/mongod.conf
  systemctl start mongod
  chconfig mongod on

  yum -y install vim htop curl wget

  #systemctl start firewalld.service
  #systemctl enable firewalld.service
  #firewall-cmd --permanent --zone=public --add-port=22/tcp
  #firewall-cmd --permanent --zone=public --add-port=27017/tcp
  #firewall-cmd --permanent --zone=public --add-port=8082/tcp

  hostnamectl set-hostname m101.vbx

  SHELL
end
