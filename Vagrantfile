# vi: ft=ruby

require 'rbconfig'

VAGRANTFILE_API_VERSION = '2'

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  # Uncomment the line with the base box of choice
  config.vm.box = 'bertvv/centos72'
  # config.vm.box = 'bertvv/fedora23'
  # config.vm.box = 'geerlingguy/centos6'
  # config.vm.box = 'geerlingguy/ubuntu1404'
  # config.vm.box = 'geerlingguy/ubuntu1604'

  config.vm.define 'testdhcp' do |node|
    node.vm.hostname = 'testdhcp'
    node.vm.network :private_network,
      ip: '192.168.222.2'
    #node.vm.network :private_network,
    #                ip: 'fdad:d155:f869:55b4::2'
    node.vm.provision 'ansible' do |ansible|
      ansible.playbook = 'test.yml'
    end
  end
  config.vm.define 'testws' do |node|
    node.vm.hostname = 'testws'
    node.vm.network :private_network,
      ip: '192.168.222.101',
      mac: '0800274CE446',
      auto_config: false
    node.vm.provision 'ansible' do |ansible|
      ansible.playbook = 'testws.yml'
    end
  end
  config.vm.define 'wsubuntu' do |node|
    node.vm.box = 'geerlingguy/ubuntu1604'
    node.vm.hostname = 'wsubuntu'
    node.vm.network :private_network,
      ip: '192.168.222.102',
      mac: '0800274CE447',
      auto_config: false
    #node.vm.provision 'ansible' do |ansible|
    #  ansible.playbook = 'testws.yml'
    #end
  end
end
