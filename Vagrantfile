# vi: ft=ruby

require 'rbconfig'

ROLE_NAME = 'dhcp'
HOST_NAME = 'test' + ROLE_NAME
VAGRANTFILE_API_VERSION = '2'

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  # Uncomment the line with the base box of choice
  config.vm.box = 'bertvv/centos72'
  # config.vm.box = 'bertvv/fedora23'
  # config.vm.box = 'geerlingguy/centos6'
  # config.vm.box = 'geerlingguy/ubuntu1404'
  # config.vm.box = 'geerlingguy/ubuntu1604'

  config.vm.define HOST_NAME do |node|
    node.vm.hostname = HOST_NAME
    node.vm.network :private_network, ip: '192.168.222.2'
    node.vm.provision 'ansible' do |ansible|
      ansible.playbook = 'test.yml'
    end
  end
end
