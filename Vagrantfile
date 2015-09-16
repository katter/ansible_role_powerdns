# -*- mode: ruby -*-
# vi: set ft=ruby :
require 'socket'

# Environment variables can be used to pass parameters
playbook_under_test = ENV['PUT'] || './powerdns.yml'
ansible_verbose_mode = ENV['AVM'] || ''

# TODO check this Vagrantfile with older versions
Vagrant.require_version ">= 1.7.0"

VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  # By default Vagrant 1.7+ is creating a private key per node.
  # Of course this adds some security, but it also breaks compatibility
  # with older versions. Since we are only using the vagrant boxes
  # for testing purposes, we will deactive this feature.
  config.ssh.insert_key = false


  # Copy and adapt this code block for multi-node setup
  config.vm.define "ubuntu_trusty" do |ubuntu_trusty|
    ubuntu_trusty.vm.box = "ubuntu/trusty64"
    ubuntu_trusty.vm.hostname = "vagrant-ubuntu"
    ## Uncomment and adapt this line if required:
    ubuntu_trusty.vm.network "forwarded_port", guest: 8001, host: 8001
    ubuntu_trusty.vm.network "forwarded_port", guest: 8000, host: 8000
  end

  config.vm.provider "virtualbox" do |v|
    v.memory = 2048
  end

  config.vm.provision "ansible" do |ansible|
    ansible.playbook = playbook_under_test
    ansible.verbose = ansible_verbose_mode

    # This should be false by default, but just to make sure:
    ansible.host_key_checking = false
    ## There are two options for usage with ansible-vault
    ## 1. Prompt for the password:
    # ansible.ask_vault_pass = true
    ## 2. Use script to get password from pe-pass
    # ansible.vault_password_file = "./.get_vault_pass_from_pe-pass"

    # The following definitions are used for the auto generated
    # inventory:
    # http://docs.vagrantup.com/v2/provisioning/ansible.html
    # Add all VMs in a multi-node setup here
    ansible.groups = {
      "ubuntu" => ["ubuntu_trusty"],
      "all_groups:children" => ["ubuntu"]
    }
  end

end
