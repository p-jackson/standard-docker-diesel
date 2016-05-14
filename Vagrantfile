Vagrant.configure(2) do |config|
  config.vm.box = "minimal/jessie64"
  config.vm.box_version = ">= 8.0, < 9.0"

  config.vm.provider "virtualbox" do |vb|
    vb.customize ["modifyvm", :id, "--usb", "off"]
    vb.customize ["modifyvm", :id, "--usbehci", "off"]
  end

  config.vm.network "forwarded_port", guest: 5000, host: 5000

  config.vm.provision "shell", inline: <<-SHELL
    sudo apt-get update -qq
    sudo apt-get install -y file libsqlite3-dev libpq-dev

    wget -nv https://drunner.s3.amazonaws.com/install_docker.sh
    sudo bash install_docker.sh
    rm install_docker.sh
    sudo adduser vagrant docker

    curl -sL https://github.com/docker/compose/releases/download/1.7.0/docker-compose-`uname -s`-`uname -m` > ./docker-compose
    sudo mv ./docker-compose /usr/local/bin/docker-compose
    sudo chmod +x /usr/local/bin/docker-compose

    curl https://sh.rustup.rs -sSf | sh -s -- -y
    source ~/.cargo/env
    cargo install diesel_cli
  SHELL
end
