# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"

node = {
#  "gitlab-c" => { :box => "centos/7", :ip => "192.168.10.44", :cpus => 1, :mem => 4024, :guest => 80, :host => 8989 },
#  "jfrog-u" => { :box => "ubuntu/trusty64", :ip => "192.168.10.11", :cpus => 2, :mem => 4024, :guest => 8081, :host => 8081 },
  "jenkins-new" => { :box => "centos/7", :ip => "192.168.10.12", :cpus => 1, :mem => 1024, :guest => 8080, :host => 8888 }
}

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  node.each_with_index do |(hostname, info), index|

    config.vm.define hostname do |cfg|
      cfg.vm.provider :virtualbox do |vb, override|
        config.vm.box = "#{info[:box]}"
        override.vm.network :private_network, ip: "#{info[:ip]}"
        override.vm.network "forwarded_port", guest: "#{info[:guest]}", host: "#{info[:host]}"
        override.vm.hostname = hostname
        vb.name = hostname
        vb.customize ["modifyvm", :id, "--memory", info[:mem], "--cpus", info[:cpus], "--hwvirtex", "on"]
      end
    end

  end

public_key = File.read(".Keys/vagrant.pub")
  config.vm.provision "shell", inline: <<-EOC
    echo '#{public_key}' >> /home/vagrant/.ssh/authorized_keys
  EOC

end
