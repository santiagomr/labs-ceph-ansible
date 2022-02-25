# Ceph lab deployed with Ansible and cephadm

This project is capable of easily and automatically initializing a laboratory Ceph cluster with N nodes (VirtualBox VMs) and OSDs with the aim of experimenting and developing on this storage technology.

To do this, it uses a multi-machine Vagrantfile and an Ansible playbook capable of installing Ceph, initializing cluster and adding OSDs through [cephadm](https://docs.ceph.com/en/latest/cephadm/index.html) (In simple words, it turns the official Ceph documentation into an Ansible playbook).

The vagrantfile includes constants to parameterize: total number of nodes (machines), total number of OSDs, OSD size, base box, IP range of the private network and number of cores and RAM assigned to each node. The ansible provisioner is invoked only when all nodes are up and running.


## Requirements

For the execution of this project it is necessary to have the following tools installed in your host:
- [VirtualBox](https://www.virtualbox.org/wiki/Downloads) (Tested on version >= 6.1.X)
- [Vagrant](https://www.vagrantup.com/downloads) (Tested on version >= 2.2.X)
- [Ansible](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html) (Tested on version >= 2.10.X)


## How to use

Once the repository is cloned, simply go to the root directory of the project.

Duplicate and rename the `vars.yml.example` file to `vars.yml`. In it you can define the variables according to what you need.

```shell
labs-ceph-ansible:(master) ✗ cp vars.yml.example vars.yml && nano vars.yml
```

Edit the constants in the `Vagrantfile` if necessary (total number of nodes, total number of OSDs, OSD size, base box, IP range of the private network and number of cores and RAM assigned to each node)

```shell
labs-ceph-ansible:(master) ✗ nano Vagrantfile
```

Finally, provision the environment:
```shell
labs-ceph-ansible:(master) ✗ export VAGRANT_EXPERIMENTAL="disks"
labs-ceph-ansible:(master) ✗ vagrant up
```

That's it! You can now log in to the Ceph web dashboard from your browser by accessing https://192.168.60.11:8443 using the defined password (`secret` by default). You can also log into your nodes via vagrant (`vagrant ssh nodeN`) or directly via SSH with the correct username and IP (`ssh ubuntu@192.168.60.1N`).


## License

GNU GPLv3


## Author Information

[@santiagomr](https://github.com/santiagomr)