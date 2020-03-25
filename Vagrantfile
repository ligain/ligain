# -*- mode: ruby -*-
# vim: set ft=ruby :

MACHINES = {
  :master => {
    :box_name => "bento/centos-8",
    :ip_addr => "192.168.11.101",
    :box_version => "202002.04.0",
    :memory => "512"
  },
  :slave => {
    :box_name => "bento/centos-8",
    :ip_addr => "192.168.11.102",
    :box_version => "202002.04.0",
    :memory => "512"
  }
}

Vagrant.configure("2") do |config|

  MACHINES.each do |boxname, boxconfig|

    config.vm.define boxname do |box|
      box.vm.box = boxconfig[:box_name]
      box.vm.box_version = boxconfig[:box_version]
      box.vm.host_name = boxname.to_s

      box.vm.network "private_network", ip: boxconfig[:ip_addr]
      box.ssh.forward_agent = true

      box.vm.provider :virtualbox do |vb|
        vb.customize ["modifyvm", :id, "--memory", boxconfig[:memory]]
      end

    end
  end

  config.vm.provision "ansible" do |ansible|
    ansible.host_vars = MACHINES
    ansible.playbook = "playbook.yml"
    ansible.become = true
    ansible.compatibility_mode = "2.0"
  end

end