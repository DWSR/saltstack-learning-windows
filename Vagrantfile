# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = '2'.freeze

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  ######
  # Config section
  ######

  cpus = 2
  memory = 2048

  img = 'bento/ubuntu-18.04'
  hyperv_switch = 'Default Switch'
  num_minions = 2
  master_ip = '172.28.181.147'

  ######
  # End config section
  ######

  config.vm.network 'private_network', bridge: hyperv_switch
  minions = (1..(num_minions)).to_a
  preseed = {}
  minions.each do |m|
    preseed["minion#{m}"] = "saltstack/keys/minion#{m}.pub"
  end

  config.vm.define 'master' do |d|
    d.vm.box = img
    d.vm.synced_folder '.', '/vagrant', disabled: true
    d.vm.synced_folder 'saltstack/salt/', '/srv/salt'
    d.vm.synced_folder 'saltstack/pillar/', '/srv/pillar'
    d.vm.provider 'hyperv' do |h|
      h.linked_clone = true
      h.vmname = "Vagrant - #{img} - Salt Master"
      h.cpus = cpus
      h.memory = memory
      h.vm_integration_services = {
        guest_service_interface: true,
        heartbeat: true,
        key_value_pair_exchange: true,
        shutdown: true,
        time_synchronization: true,
        vss: true
      }
    end
    d.vm.provision :salt do |s|
      s.install_master = true
      s.install_type = 'stable'
      s.master_key = 'saltstack/keys/master.pem'
      s.master_pub = 'saltstack/keys/master.pub'
      s.seed_master = preseed
      s.no_minion = true
      s.verbose = true
      s.colorize = true
      s.bootstrap_options = '-P -c /tmp -x python3'
    end
  end

  minions.each do |idx|
    config.vm.define "minion#{idx}" do |d|
      d.vm.box = img
      d.vm.synced_folder '.', '/vagrant', disabled: true
      d.vm.synced_folder 'saltstack/salt/', '/srv/salt'
      d.vm.synced_folder 'saltstack/pillar/', '/srv/pillar'
      d.vm.provider 'hyperv' do |h|
        h.linked_clone = true
        h.vmname = "Vagrant - #{img} - Salt Minion #{idx}"
        h.cpus = cpus
        h.memory = memory
        h.vm_integration_services = {
          guest_service_interface: true,
          heartbeat: true,
          key_value_pair_exchange: true,
          shutdown: true,
          time_synchronization: true,
          vss: true
        }
      end
      d.vm.provision :salt do |s|
        s.install_type = 'stable'
        s.minion_key = "saltstack/keys/minion#{idx}.pem"
        s.minion_pub = "saltstack/keys/minion#{idx}.pub"
        s.minion_json_config = %Q({ "master": "#{master_ip}", "id": "minion#{idx}" })
      end
    end
  end
end
