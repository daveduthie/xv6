# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/xenial64"

  config.vm.provider 'virtualbox' do |vb|
     vb.customize [ "guestproperty", "set", :id, "/VirtualBox/GuestAdd/VBoxService/--timesync-set-threshold", 1000 ]
  end

  config.vm.provision "shell", inline: <<-SHELL
    apt-get update
    apt-get install -y build-essential \
                       gcc-multilib \
                       git \
                       vim \
                       gdb \
                       tmux \
                       python \
                       tree \
                       pkg-config \
                       zlib1g-dev \
                       libglib2.0-dev \
                       libpixman-1-dev \
                       flex \
                       bison

    if [ ! -d "/home/vagrant/qemu" ]; then
      git clone http://web.mit.edu/ccutler/www/qemu.git -b 6.828-2.3.0 /home/vagrant/qemu
    fi

    cd /home/vagrant/qemu
    git submodule init
    git submodule update --recursive
    git submodule status --recursive
    ./configure --target-list="i386-softmmu"
    make -j4 && make -j4 install
    cd /home/vagrant
    echo "Done!"
  SHELL

  config.vm.synced_folder ".", "/home/vagrant/lab"
  config.ssh.forward_x11 = true
end
