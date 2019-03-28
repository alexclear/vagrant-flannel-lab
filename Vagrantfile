# -*- mode: ruby -*-
# vi: set ft=ruby :

$FLANNEL1_IP = "172.16.137.142"
$FLANNEL2_IP = "172.16.137.143"
$FLANNEL3_IP = "172.16.137.144"

ANSIBLE_RAW_SSH_ARGS = []

Vagrant.configure("2") do |config|
  config.vm.define "flannel1" do |flannel1|
    flannel1.vm.box = "generic/ubuntu1804"
    flannel1.vm.hostname = "flannel1"
    flannel1.vm.network "private_network", ip: $FLANNEL1_IP

    flannel1.vm.provider :virtualbox do |v, override|
      v.gui = false
      v.customize ["modifyvm", :id, "--cpus", 2]
      v.customize ["modifyvm", :id, "--memory", 4096]
      v.customize ["modifyvm", :id, "--cableconnected1", "on"]
      v.customize ["modifyvm", :id, "--cableconnected2", "on"]
    end

    flannel1.vm.provider :libvirt do |v, override|
      v.cpu_mode = 'custom'
      v.cpu_model = 'kvm64'
      v.volume_cache = 'writeback'
      v.disk_bus = 'virtio'
      v.cpus = 2
      v.memory = 4096
    end

    flannel1.vm.provision "shell", inline: "apt-get install -y python"
  end

  config.vm.define "flannel2" do |flannel2|
    flannel2.vm.box = "generic/ubuntu1804"
    flannel2.vm.hostname = "flannel2"
    flannel2.vm.network "private_network", ip: $FLANNEL2_IP

    flannel2.vm.provider :virtualbox do |v, override|
      v.gui = false
      v.customize ["modifyvm", :id, "--cpus", 2]
      v.customize ["modifyvm", :id, "--memory", 4096]
      v.customize ["modifyvm", :id, "--cableconnected1", "on"]
      v.customize ["modifyvm", :id, "--cableconnected2", "on"]
    end

    flannel2.vm.provider :libvirt do |v, override|
      v.cpu_mode = 'custom'
      v.cpu_model = 'kvm64'
      v.volume_cache = 'writeback'
      v.disk_bus = 'virtio'
      v.cpus = 2
      v.memory = 4096
    end

    flannel2.vm.provision "shell", inline: "apt-get install -y python"
  end

  config.vm.define "flannel3" do |flannel3|
    flannel3.vm.box = "generic/ubuntu1804"
    flannel3.vm.hostname = "flannel3"
    flannel3.vm.network "private_network", ip: $FLANNEL3_IP

    flannel3.vm.provider :virtualbox do |v, override|
      ANSIBLE_RAW_SSH_ARGS << " -o IdentityFile=./.vagrant/machines/flannel1/virtualbox/private_key "
      ANSIBLE_RAW_SSH_ARGS << " -o IdentityFile=./.vagrant/machines/flannel2/virtualbox/private_key "
      ANSIBLE_RAW_SSH_ARGS << " -o IdentityFile=./.vagrant/machines/flannel3/virtualbox/private_key "
      v.gui = false
      v.customize ["modifyvm", :id, "--cpus", 2]
      v.customize ["modifyvm", :id, "--memory", 4096]
      v.customize ["modifyvm", :id, "--cableconnected1", "on"]
      v.customize ["modifyvm", :id, "--cableconnected2", "on"]
    end

    flannel3.vm.provider :libvirt do |v, override|
      ANSIBLE_RAW_SSH_ARGS << " -o IdentityFile=./.vagrant/machines/flannel1/libvirt/private_key "
      ANSIBLE_RAW_SSH_ARGS << " -o IdentityFile=./.vagrant/machines/flannel2/libvirt/private_key "
      ANSIBLE_RAW_SSH_ARGS << " -o IdentityFile=./.vagrant/machines/flannel3/libvirt/private_key "
      v.cpu_mode = 'custom'
      v.cpu_model = 'kvm64'
      v.volume_cache = 'writeback'
      v.disk_bus = 'virtio'
      v.cpus = 2
      v.memory = 4096
    end

    flannel3.vm.provision "shell", inline: "apt-get install -y python"
    flannel3.vm.provision "ansible" do |ansible|
      ansible.playbook = "ansible/site.yml"
      ansible.config_file = "ansible/ansible.cfg"
      ansible.inventory_path = "ansible/inventory"
      ansible.become = true
      ansible.limit = "all"
      ansible.raw_ssh_args = ANSIBLE_RAW_SSH_ARGS
    end
  end
end
