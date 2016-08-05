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
  config.vm.box = "debian/jessie64"

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

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  #
  config.vm.provider "virtualbox" do |vb|
    # Display the VirtualBox GUI when booting the machine
    # vb.gui = true

    # Customize the amount of memory on the VM:
    vb.memory = "2048"
  end
  #
  # View the documentation for the provider you are using for more
  # information on available options.

  # Define a Vagrant Push strategy for pushing to Atlas. Other push strategies
  # such as FTP and Heroku are also available. See the documentation at
  # https://docs.vagrantup.com/v2/push/atlas.html for more information.
  # config.push.define "atlas" do |push|
  #   push.app = "YOUR_ATLAS_USERNAME/YOUR_APPLICATION_NAME"
  # end

  # Enable provisioning with a shell script. Additional provisioners such as
  # Puppet, Chef, Ansible, Salt, and Docker are also available. Please see the
  # documentation for more information about their specific syntax and use.
  #
  # Login as vagrant with {privileged: false}
  # see http://qiita.com/pasela/items/906291647c4f97b9a7c7
  config.vm.provision "shell", inline: <<-SHELL
    apt-get update

    # Set the Server Timezone to Tokyo
    echo "Asia/Tokyo" > /etc/timezone
    dpkg-reconfigure -f noninteractive tzdata

    # See https://gist.github.com/sheikhwaqas/9088872
    echo "mysql-server mysql-server/root_password password rootpw"       | debconf-set-selections
    echo "mysql-server mysql-server/root_password_again password rootpw" | debconf-set-selections
    apt-get -y install mysql-server
    mysqladmin -u root --password=rootpw password ''

    apt-get -y install emacs-nox git build-essential automake zlib1g-dev \
        libssl-dev libreadline6-dev libyaml-dev libxml2-dev libxslt-dev libffi-dev \
        libcurl4-openssl-dev libmysqlclient-dev libxslt1-dev autoconf libncurses5-dev \
        bison curl
  SHELL

  config.vm.provision "shell", privileged: false, inline: <<-SHELL
    git clone https://github.com/sstephenson/rbenv.git              ~/.rbenv
    git clone https://github.com/sstephenson/ruby-build.git         ~/.rbenv/plugins/ruby-build
    git clone https://github.com/sstephenson/rbenv-default-gems.git ~/.rbenv/plugins/rbenv-default-gems

    echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bash_profile
    echo 'eval "$(rbenv init -)"'               >> ~/.bash_profile

    echo 'rbenv-rehash' >> ~/.rbenv/default-gems
    echo 'bundler'      >> ~/.rbenv/default-gems

    PS1='$ '
    source ~/.bash_profile

    # http://qiita.com/cock1doodledoo/items/99414548e25d63a398c5
    # set interactive environmental-valiables
    LANG=C rbenv install 2.3.1
    rbenv global 2.3.1

    git clone https://github.com/riywo/ndenv ~/.ndenv
    echo 'export PATH="$HOME/.ndenv/bin:$PATH"' >> ~/.bash_profile
    echo 'eval "$(ndenv init -)"'               >> ~/.bash_profile
    # exec $SHELL -l
    source ~/.bash_profile
    git clone https://github.com/riywo/node-build.git $(ndenv root)/plugins/node-build

    ndenv install v5.11.1
    ndenv global v5.11.1
    ndenv rehash
  SHELL

  # # After `vagrant up`, do the following commands
  #
  # local$ vagrant ssh default -- -A
  # vagrant@debian-jessie:~$ git clone git@github.com:ms-development/msa_server.git
  # vagrant@debian-jessie:~$ cd msa_server/
  # vagrant@debian-jessie:~/msa_server$ bin/setup
  # vagrant@debian-jessie:~/msa_server$ bin/rake spec

end
