VAGRANTFILE_API_VERSION = "2"
file_to_disk1 = './disk-0-1.qcow2'
file_to_disk2 = './disk-0-2.qcow2'

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.ssh.insert_key = false
  config.vm.box_check_update = false
  config.vm.synced_folder '.', '/vagrant', type: "rsync", rsync__exclude: [".vagrant/", ".git/", "*.qcow2", ".github"]

  # ========== Server2 ==========
  config.vm.define "server2" do |server2|
    server2.vm.box = "generic/rocky9"
    server2.vm.hostname = "server2.nine.example.com"

    server2.vm.network "private_network", ip: "192.168.55.151", libvirt__network_name: "rhcsa_network"
    server2.vm.network "private_network", ip: "192.168.55.175", libvirt__network_name: "rhcsa_network"
    server2.vm.network "private_network", ip: "192.168.55.176", libvirt__network_name: "rhcsa_network"

    server2.vm.provider :libvirt do |lv|
      lv.memory = 4096
      lv.cpus = 2
      lv.disk_bus = 'virtio'
      lv.storage :file, size: '20G', device: 'vdb', type: 'qcow2'
      lv.storage :file, size: '20G', device: 'vdc', type: 'qcow2'
    end

    # Formatear y montar discos
    server2.vm.provision "shell", inline: <<-SHELL
      mkfs.ext4 -L extradisk1 /dev/vdb || true
      mkdir -p /extradisk1
      grep -q 'LABEL=extradisk1' /etc/fstab || echo 'LABEL=extradisk1 /extradisk1 ext4 defaults 0 0' >> /etc/fstab
    SHELL

    server2.vm.provision "shell", inline: <<-SHELL
      mkfs.ext4 -L extradisk2 /dev/vdc || true
      mkdir -p /extradisk2
      grep -q 'LABEL=extradisk2' /etc/fstab || echo 'LABEL=extradisk2 /extradisk2 ext4 defaults 0 0' >> /etc/fstab
    SHELL

    server2.vm.provision :ansible_local do |ansible|
      ansible.playbook = "/vagrant/playbooks/server2.yml"
      ansible.compatibility_mode = "2.0"
      ansible.inventory_path = "/vagrant/inventory"
      ansible.config_file = "/vagrant/ansible.cfg"
      ansible.extra_vars = { ansible_connection: "local" }
    end
  end

  # ========== Repo ==========
  config.vm.define "repo" do |repo|
    repo.vm.box = "generic/rocky9"
    repo.vm.hostname = "repo.nine.example.com"
    repo.vm.network "private_network", ip: "192.168.55.149", libvirt__network_name: "rhcsa_network"

    repo.vm.provider :libvirt do |lv|
      lv.memory = 2048
      lv.cpus = 1
    end

    repo.vm.provision :ansible_local do |ansible|
      ansible.playbook = "/vagrant/playbooks/repo.yml"
      ansible.compatibility_mode = "2.0"
      ansible.inventory_path = "/vagrant/inventory"
      ansible.config_file = "/vagrant/ansible.cfg"
      ansible.extra_vars = { ansible_connection: "local" }
    end
  end

  # ========== Server1 ==========
  config.vm.define "server1" do |server1|
    server1.vm.box = "generic/rocky9"
    server1.vm.hostname = "server1.nine.example.com"
    server1.vm.network "private_network", ip: "192.168.55.150", libvirt__network_name: "rhcsa_network"

    server1.vm.provider :libvirt do |lv|
      lv.memory = 2048
      lv.cpus = 2
    end

    server1.vm.provision :ansible_local do |ansible|
      ansible.playbook = "/vagrant/playbooks/master.yml"
      ansible.compatibility_mode = "2.0"
      ansible.inventory_path = "/vagrant/inventory"
      ansible.config_file = "/vagrant/ansible.cfg"
      ansible.extra_vars = { ansible_connection: "local" }
    end
  end
end

