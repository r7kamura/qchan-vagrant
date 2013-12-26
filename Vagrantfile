Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu-13.04-amd64"
  config.vm.box_url = "http://cloud-images.ubuntu.com/vagrant/raring/current/raring-server-cloudimg-amd64-vagrant-disk1.box"

  config.vm.provider :virtualbox do |provider, override|
    override.vm.provision :shell, inline: <<-EOS.gsub(/^ {6}/, "")
      # Install packages
      wget -q -O - https://get.docker.io/gpg | apt-key add -
      echo 'deb http://get.docker.io/ubuntu docker main' > /etc/apt/sources.list.d/docker.list
      apt-get update -qq
      apt-get install -y lxc-docker
      apt-get install -y \
        autoconf \
        bison \
        build-essential \
        curl \
        git \
        git-core \
        libncurses5-dev \
        libreadline6-dev \
        libsqlite3-dev \
        libssl-dev \
        libxml2-dev \
        libxslt1-dev \
        libyaml-dev \
        lxc-docker \
        redis-server \
        ruby-dev \
        sqlite3 \
        ssh \
        zlib1g-dev

      # Install MySQL & setup root user
      echo 'mysql-server-5.5 mysql-server/root_password password password' | debconf-set-selections
      echo 'mysql-server-5.5 mysql-server/root_password_again password password' | debconf-set-selections
      apt-get install -y \
        libmysql-ruby \
        libmysqlclient-dev \
        mysql-server

      # Set up MySQL user
      export MYSQL_PWD=password
      mysql -u root -e "grant all privileges on *.* to root@'127.0.0.1';"
    EOS

    override.vm.provision :shell, privileged: false, inline: <<-EOS.gsub(/^ {6}/, "")
      # Install Ruby
      git clone git://github.com/sstephenson/rbenv.git .rbenv
      git clone https://github.com/sstephenson/ruby-build.git ~/.rbenv/plugins/ruby-build
      echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> .bash_profile
      which rbenv || echo 'eval "$(rbenv init -)"' >> .bash_profile
      source .bash_profile
      rbenv install 2.1.0
      rbenv rehash
      rbenv global 2.1.0
      gem install bundler
      rbenv rehash

      # Set up Qchan Worker
      git clone https://github.com/r7kamura/qchan-worker.git
      cd qchan-worker && bundle install
      cd ..

      # Set up Qchan API
      git clone https://github.com/r7kamura/qchan-api.git
      cd qchan-api && bundle install
      cd ..

      # Set up Qchan Vagrant
      git clone https://github.com/r7kamura/qchan-vagrant.git
      cd qchan-vagrant && bundle install
      cd ..

      # Done
      echo 'Done provisioning.'
    EOS
  end
end
