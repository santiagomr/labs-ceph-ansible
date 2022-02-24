NODES = 3
NODE_BOX = "ubuntu/focal64"
NODE_CPUS = 2
NODE_RAM = 2048
NODE_PRIVNET_PREFIX = "192.168.60."

OSD_PER_NODE = 3
OSD_SIZE = "16GB"

Vagrant.configure("2") do |config|

  config.vm.box = NODE_BOX

  config.vm.provider "virtualbox" do |vb|
    vb.cpus = NODE_CPUS
    vb.memory = NODE_RAM
  end

  # Need: export VAGRANT_EXPERIMENTAL="disks"
  (1..OSD_PER_NODE).each do |i|
    config.vm.disk :disk, name: "osd#{i}", size: OSD_SIZE
  end

  ANSIBLE_RAW_SSH_ARGS = []
  (1..NODES-1).each do |i|
    ANSIBLE_RAW_SSH_ARGS << "-o IdentityFile=.vagrant/machines/node#{i}/virtualbox/private_key"
  end

  (1..NODES).each do |i|
    config.vm.define "node#{i}" do |node|
      node.vm.hostname = "node#{i}"
      node.vm.network "private_network", ip: "#{NODE_PRIVNET_PREFIX}#{i+10}"

      if i == NODES # Run the Ansible provisione once the creation of all VMs is finished
        node.vm.provision :ansible do |ansible|
          ansible.raw_ssh_args = ANSIBLE_RAW_SSH_ARGS
          ansible.inventory_path = "hosts"
          ansible.limit = "all"
          ansible.playbook = "site.yml"
          ansible.verbose = "-v"
        end
      end
    end
  end
end
