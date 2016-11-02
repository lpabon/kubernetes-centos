# -*- mode: ruby -*-
# vi: set ft=ruby :

NODES = 3
DISKS = 3

Vagrant.configure("2") do |config|
    config.ssh.insert_key = false
    config.vm.box = "centos/7"

    # Override
    config.vm.provider :libvirt do |v,override|
        override.vm.synced_folder '.', '/home/vagrant/sync', disabled: true
    end

    # Make kub master
    config.vm.define :master do |master|
        master.vm.host_name = "master"

        master.vm.provider :libvirt do |lv|
            lv.memory = 1024
            lv.cpus = 2
        end

    end

    # Make the glusterfs cluster, each with DISKS number of drives
    (0..NODES-1).each do |i|
        config.vm.define "node#{i}" do |node|
            node.vm.hostname = "node#{i}"

            (0..DISKS-1).each do |d|
                driverletters = ('b'..'z').to_a
                node.vm.provider :libvirt do  |lv|
                    lv.storage :file, :device => "vd#{driverletters[d]}", :path => "atomic-disk-#{i}-#{d}.disk", :size => '1024G'
                    lv.memory = 1024
                    lv.cpus =2
                end
            end

            if i == (NODES-1)
                # View the documentation for the provider you're using for more
                # information on available options.
                node.vm.provision :ansible do |ansible|
                    ansible.limit = "all"
                    ansible.playbook = "site.yml"
                    ansible.groups = {
                        "master" => ["master"],
                        "nodes" => (0..NODES-1).map {|j| "node#{j}"},
                    }
                end
            end
        end
    end
end
