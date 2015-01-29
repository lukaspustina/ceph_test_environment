# Ceph Test Environment
A Vagrant based Ceph Test Environment

## Setup Vagrant

1. Install [VirtualBox](https://www.virtualbox.org) and [Vagrant](https://www.vagrantup.com)
1. `vagrant box add ubuntu/trusty64`
1. `vagrant up`

## Example Installation of Ceph

All commands are executed as `root` on `alpha`, i.e. `vagrant ssh alpha`

### Install Ceph -- Version *Firefly*

1. `mkdir ceph`
1. `cd ceph`
1. `ceph-deploy install --release firefly alpha beta gamma`
1. `for i in alpha beta gamma; do echo -n "$i "; ssh $i ceph --version; done`
1. `ceph-deploy new alpha beta gamma`
1. Adapt `ceph.conf` for additional parameters
1. `ceph-deploy --overwrite-conf mon create alpha beta gamma`
1. `ceph -c ceph.conf quorum_status`
1. `ceph -c ceph.conf -s`
1. `ceph-deploy gatherkeys alpha beta gamma`
1. `ceph-deploy disk list <host>` with `host` in [`alpha`, `beta`, `gamma`]
1. For the disk you want to add: `ceph-deploy osd create <host>:<disk>` with `disk` from the the previous step
1. `ceph -s`
1. `ceph osd tree`

### Install CephFS
1. `ceph-deploy mds alpha beta gamma`
1. `mount.ceph alpha,beta,gamma:/ /mnt -o name=admin,secret=$(ceph-authtool -p /etc/ceph/ceph.client.admin.keyring)`

