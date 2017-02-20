# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|
  config.vm.box = 'ubuntu/xenial64'

  config.vm.provider 'virtualbox' do |vb|
    vb.memory = '2048'
  end

  config.vm.synced_folder './share', '/home/vagrant/share', create: true

  config.vm.provision 'shell', privileged: false, inline: <<-SHELL
    # change to use JAIST mirror server
    if grep '//ftp.jaist.ac.jp/pub/Linux' /etc/apt/sources.list
    then
      echo 'already used JAIST mirror server'
    else
      sudo sed -i.bak -e 's|//archive.ubuntu.com|//ftp.jaist.ac.jp/pub/Linux|' /etc/apt/sources.list
    fi

    # update
    sudo apt-get update
    sudo apt-get --yes upgrade

    # install dependencies
    sudo apt-get install --yes build-essential g++

    # install Git
    sudo apt-get install --yes git

    # create symlink for share
    ln -s /vagrant $HOME/share

    # fast `git status`
    sudo git config --system core.preloadindex true
    sudo git config --system core.fscache true

    # show multibyte filename
    # "\346\227\245\346\234\254\350\252\236OK.xlsx" -> 日本語OK.xls
    sudo git config --system core.quotepath false

    # install nodebrew
    curl -fsSL git.io/nodebrew | perl - setup

    # add PATH
    export PATH=$HOME/.nodebrew/current/bin:$PATH

    # add PATH to .bashrc
    echo 'export PATH=$HOME/.nodebrew/current/bin:$PATH' >> $HOME/.bashrc

    # install node.js, progress send to /dev/null
    nodebrew install-binary stable 2>/dev/null

    # set node.js
    nodebrew use stable

    # install task runner modules
    npm install -g grunt-cli gulp-cli
  SHELL
end
