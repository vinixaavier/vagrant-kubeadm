# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = 2

require 'yaml'

# Load environment yaml file
environment = YAML.load_file('environment.yaml')

# Store all global settings in variables
global_settings = environment['global']
linked_clone = global_settings['linked_clone']
landrush = global_settings['landrush']
domain = global_settings['domain']
network_prefix = global_settings['network_prefix']
global_memory = global_settings['memory']
global_cpus = global_settings['cpus']
global_box = global_settings['box']

# Store all hosts
hosts = environment['hosts']

# Checking if latest release version of Vagrant
Vagrant.require_version '>= 2.0.2'

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  # Activating landrush plugin if enabled
  if Vagrant.has_plugin?('landrush') && landrush
    config.landrush.enabled = true
    config.landrush.tld = domain
  end

  config.vm.synced_folder '.', '/vagrant', type: 'nfs'

  # Loop to provisioning all hosts in environment yaml file
  hosts.each_with_index do |host, index|
    # Store all environment host settings in variables
    hostname = host['hostname']
    box = host['box']
    cpus = host['cpus']
    memory = host['memory']
    provision = host['provision']

    # Settings of each host
    config.vm.define hostname do |define|
      # Checking if is global settings or host settings
      if box then define.vm.box = box else define.vm.box = global_box end

      define.vm.hostname = "#{hostname}.#{domain}"
      define.vm.network 'private_network', ip: "#{network_prefix}.#{index + 10}"

      # Settings of each host in VirtualBox 
      define.vm.provider 'virtualbox' do |virtualbox|
        virtualbox.name = hostname

        # Checking if is global settings or host settings
        if cpus then virtualbox.cpus = cpus else virtualbox.cpus = global_cpus end
        if memory then virtualbox.memory = memory else virtualbox.memory = global_memory end
        if linked_clone then virtualbox.linked_clone = true end
      end

      define.vm.provision :hosts, :sync_hosts => true

      # Checking if have provisioning tools
      if provision == 'ansible'
        define.vm.provision :ansible_local do |ansible|
          ansible.playbook = host['playbook']
          ansible.install_mode = :pip
          ansible.compatibility_mode = '2.0'
          ansible.become = true
          ansible.become_user = 'root'
        end
      end
    end
  end
end
