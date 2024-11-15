Vagrant.configure("2") do |config|

  config.vm.box = "base"

  config.vm.box = "debian/bookworm64"

  config.vm.provision "shell", inline: <<-SHELL
    apt-get update
    apt-get install -y bind9
  SHELL

  config.vm.define "nginx" do |venus|
    venus.vm.network "private_network", ip: "192.168.57.102"
    venus.vm.provision "shell", inline: <<-SHELL
    sudo su
    cp -v /vagrant/named.conf.local.venus /etc/bind/named.conf.local
    systemctl restart bind9
    SHELL
  end

end
