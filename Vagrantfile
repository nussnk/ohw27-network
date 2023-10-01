# -*- mode: ruby -*-
# vim: set ft=ruby :
# -*- mode: ruby -*-
# vim: set ft=ruby :

MACHINES = {
:inetRouter => {
        :box_name => "centos/7",
        :prov_file => "ansible/inetRouter.yml",
        #:public => {:ip => '10.10.10.1', :adapter => 1},
        :net => [
                   {ip: '192.168.255.1', adapter: 2, netmask: "255.255.255.252", virtualbox__intnet: "Con-Inet-Central"},
                   {ip: '192.168.50.100', adapter: 8},
                ]
  },

  :centralRouter => {
        :box_name => "centos/7",
        :prov_file => "ansible/centralRouter.yml",
        :net => [
                   {ip: '192.168.255.2', adapter: 2, netmask: "255.255.255.252", virtualbox__intnet: "Con-Inet-Central"},
                   {ip: '192.168.10.1', adapter: 3, netmask: "255.255.255.240", virtualbox__intnet: "Central-DIR-net"},
                   {ip: '192.168.10.33', adapter: 4, netmask: "255.255.255.240", virtualbox__intnet: "Central-HW-net"},
                   {ip: '192.168.10.65', adapter: 5, netmask: "255.255.255.192", virtualbox__intnet: "Central-Wi-Fi-net"},
                   {ip: '192.168.255.9', adapter: 6, netmask: "255.255.255.252", virtualbox__intnet: "test-net"},
                   {ip: '192.168.255.5', adapter: 7, netmask: "255.255.255.252", virtualbox__intnet: "Con-Central-Office2-net"},
                   {ip: '192.168.50.101', adapter: 8},
                ]
  },
  
  :centralServer => {
        :box_name => "centos/7",
        :prov_file => "ansible/centralServer.yml",
        :net => [
                   {ip: '192.168.10.2', adapter: 2, netmask: "255.255.255.240", virtualbox__intnet: "Central-DIR-net"},
                   {ip: '192.168.50.102', adapter: 8},
                   #{adapter: 3, auto_config: false, virtualbox__intnet: true},
                   #{adapter: 4, auto_config: false, virtualbox__intnet: true},
                ]
  },

  :office1Router => {
        :box_name => "ubuntu/focal64",
        :prov_file => "ansible/office1Router.yml",
        :net => [
                   {ip: '192.168.255.10', adapter: 2, netmask: "255.255.255.252", virtualbox__intnet: "test-net"},
                   {ip: '192.168.2.1', adapter: 3, netmask: "255.255.255.192", virtualbox__intnet: "Office1-Dev-net"},
                   {ip: '192.168.2.65', adapter: 4, netmask: "255.255.255.192", virtualbox__intnet: "Office1-Test-net"},
                   {ip: '192.168.2.129', adapter: 5, netmask: "255.255.255.192", virtualbox__intnet: "Office1-Managers-net"},
                   {ip: '192.168.2.193', adapter: 6, netmask: "255.255.255.192", virtualbox__intnet: "Office1-HW-net"},
                   {ip: '192.168.50.111', adapter: 8},
                ]
  },
 
  :office1Server => {
        :box_name => "ubuntu/focal64",
        :prov_file => "ansible/office1Server.yml",
        :net => [
                   {ip: '192.168.2.130', adapter: 2, netmask: "255.255.255.192", virtualbox__intnet: "Office1-Managers-net"},
                   {ip: '192.168.50.112', adapter: 8},
                ]
  }, 
  
  :office2Router => {
        :box_name => "debian/bullseye64",
        :prov_file => "ansible/office2Router.yml",
        :net => [
                   {ip: '192.168.255.6', adapter: 2, netmask: "255.255.255.252", virtualbox__intnet: "Con-Central-Office2-net"},
                   {ip: '192.168.1.1', adapter: 3, netmask: "255.255.255.192", virtualbox__intnet: "Office2-Dev-net"},
                   {ip: '192.168.1.129', adapter: 4, netmask: "255.255.255.192", virtualbox__intnet: "Office2-Test-net"},
                   {ip: '192.168.1.193', adapter: 5, netmask: "255.255.255.192", virtualbox__intnet: "Office2-HW-net"},
                   {ip: '192.168.50.121', adapter: 8},
                ]
  },
 
  :office2Server => {
        :box_name => "debian/bullseye64",
        :prov_file => "ansible/office2Server.yml",
        :net => [
                   {ip: '192.168.1.2', adapter: 2, netmask: "255.255.255.192", virtualbox__intnet: "Office2-Dev-net"},
                   {ip: '192.168.50.122', adapter: 8},
                ]
  }, 
}

Vagrant.configure("2") do |config|

  MACHINES.each do |boxname, boxconfig|

    config.vm.define boxname do |box|

        box.vm.box = boxconfig[:box_name]
        box.vm.host_name = boxname.to_s

        boxconfig[:net].each do |ipconf|
          box.vm.network "private_network", ip: ipconf[:ip], netmask: ipconf[:netmask], adapter: ipconf[:adapter], virtualbox__intnet: ipconf[:virtualbox__intnet]
        end
        
        if boxconfig.key?(:public)
          box.vm.network "public_network", boxconfig[:public]
        end

        box.vm.provision "shell", inline: <<-SHELL
          mkdir -p ~root/.ssh
                cp ~vagrant/.ssh/auth* ~root/.ssh
        SHELL
        
        box.vm.provision "ansible" do |ansible|
            ansible.playbook = boxconfig[:prov_file]
            ansible.inventory_path = "ansible/hosts"
            ansible.host_key_checking = "false"
            ansible.verbose = "v"
            ansible.limit = "all"
        end

      end

  end
  
  
end

