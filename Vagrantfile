# vi: ft=ruby

require 'rbconfig'

ROLE_NAME = 'dhcp'
VAGRANTFILE_API_VERSION = '2'

distros = [
  { name: 'centos', box: 'bento/centos-7.6' },
  { name: 'fedora', box: 'bento/fedora-30' },
  { name: 'ubuntu', box: 'bento/ubuntu-18.04' }
]

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  distros.each do |distro|
    srv_name = 'srv' + distro[:name]
    config.vm.define srv_name do |node|
      node.vm.hostname = srv_name
      node.vm.box = distro[:box]

      node.vm.network :private_network,
        ip: '192.168.222.2'

      node.vm.provision 'ansible' do |ansible|
        ansible.compatibility_mode = '2.0'
        ansible.playbook = 'test.yml'
      end
    end
  end
end

