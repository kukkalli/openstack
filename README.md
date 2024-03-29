# OpenStack Installation Guide

## Initial Setup
[SSH Configuration](initial-setup/ssh.md#setup-ssh-keys)

## Environment Setup
[Host Networking](environment-setup/host-networking.md#host-networking)

[Network Time Protocol (NTP)](environment-setup/ntp.md#network-time-protocol-ntp)

[OpenStack packages](environment-setup/packages.md#openstack-packages)

[SQL Database](environment-setup/sql.md#sql-database)

[Message Queue](environment-setup/rabbitmq.md#message-queue)

[Memcached](environment-setup/memcached.md#memcached)

[Etcd](environment-setup/etcd.md#etcd)

## Install OpenStack services

### Identity Service
#### Standalone setup
[Keystone](services/controller/keystone.md#keystone-identity-service)

### Image Service
#### Standalone setup
[Glance](services/controller/glance.md#glance-image-service)

### Placement Service
#### Standalone setup
[Placement](services/controller/placement.md#placement-service)

### Compute Service
#### Standalone setup
[Nova](services/nova.md#nova-compute-service)

### Networking Service
#### Standalone setup
[Neutron](services/neutron.md#neutron-networking-service)

### OpenStack Dashboard
[Horizon](services/controller/horizon.md#openstack-dashboard-horizon)

## Create Virtual Machines
[Create VMs](services/create-vm.md#create-vms-on-cli)

# OpenDaylight Setup for OpenStack
[Install OpenDaylight](opendaylight.md)