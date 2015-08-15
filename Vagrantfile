# -*- mode: ruby -*-
# vi: set ft=ruby :
Vagrant.require_version '>= 1.7.4'

ansible_groups = {
  "group-osm-tile-cache"    => ["osm-tile-cache-01",    "osm-tile-cache-01"    ],
  "group-osm-tile-render"   => ["osm-tile-render-01",   "osm-tile-render-01"   ],
  "group-osm-tile-database" => ["osm-tile-database-01", "osm-tile-database-01" ],
}

Vagrant.configure(2) do |config|
  config.vm.box = "ubuntu/trusty64"

  config.vm.synced_folder ".", "/Vagrant", disabled: true

  config.vm.provider "virtualbox" do |vb|
    vb.gui = false
    vb.memory = "1024"
    vb.cpus   = 4
    # paravirtprovider is only support in Virtualbox 5.0+
    vb.customize ["modifyvm", :id, "--paravirtprovider",    "kvm"]
    vb.customize ["modifyvm", :id, "--chipset",             "ich9"]
    vb.customize ["modifyvm", :id, "--nictype1",            "virtio"]
    vb.customize ["modifyvm", :id, "--nictype2",            "virtio"]
    vb.customize ["modifyvm", :id, "--vram",                "12"]
    vb.customize ["modifyvm", :id, "--hpet",                "on"]
    vb.customize ["modifyvm", :id, "--pae",                 "on"]
    vb.customize ["modifyvm", :id, "--acpi",                "on"]
    vb.customize ["modifyvm", :id, "--ioapic",              "on"]
    vb.customize ["modifyvm", :id, "--usb",                 "off"]
    vb.customize ["modifyvm", :id, "--usbehci",             "off"]
  end

  config.vm.define 'osm-tile-cache-01', autostart: true do |box|
    box.vm.hostname  = 'osm-tile-cache-01'
    box.vm.network "private_network", ip: "10.242.100.10", netmask: '255.255.255.0', nictype: 'virtio'
  end

  config.vm.define 'osm-tile-render-01', autostart: true do |box|
    box.vm.hostname  = 'osm-tile-render-01'
    box.vm.network "private_network", ip: "10.242.100.110", netmask: '255.255.255.0', nictype: 'virtio'
  end

  config.vm.define 'osm-tile-database-01', autostart: true do |box|
    box.vm.hostname  = 'osm-tile-database-01'
    box.vm.network "private_network", ip: "10.242.100.210", netmask: '255.255.255.0', nictype: 'virtio'

    box.vm.provision :ansible do |ansible|
      ansible.playbook = "site.yml"
      ansible.sudo = true
      ansible.host_key_checking = false
      ansible.limit = 'all'
      ansible.groups = ansible_groups
    end
  end
end
