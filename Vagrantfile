# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = "Centos 6.4 x64"
  config.vm.box_url = 'http://developer.nrel.gov/downloads/vagrant-boxes/CentOS-6.4-x86_64-v20130427.box'
  config.vm.synced_folder "#{ENV['HOME']}/dev/artifacts/", "/net/devhi/IBM_Installs/_software/puppet/_software", nfs: false

  # Bootstraps the Dev environemnt
  config.vm.define 'dev' do |dev|
    vm_ip = '192.168.57.100'
    dev.vm.hostname = 'amp.dev'

    # binds a static private IP to the master VM (not reachable externaly)
    dev.vm.network "private_network", ip: vm_ip
    dev.vm.network "forwarded_port", guest: 8080, host: 8081

    # Customize VirtualBox parameters
    dev.vm.provider "virtualbox" do |vb|
      vb.name = "amp-puppet"
      vb.customize ["modifyvm", :id,
                    "--memory", 8192,
                    "--vram", 10,
                    "--hwvirtex", "on"]
    end

    # Setup Puppet as environment provisioner
    dev.vm.provision :puppet do |puppet|
      puppet.facter = {
        ipaddress: vm_ip
      }
      puppet.options = "--verbose --debug"
      puppet.module_path = ['modules', 'mymodule']
      puppet.manifests_path = '.'
      puppet.manifest_file  = 'site.pp'
    end
  end
end
