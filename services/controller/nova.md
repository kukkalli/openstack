## Install and configure controller node

### Prerequisites
#### Use the database access client to connect to the database server as the ```root``` user
```
# mysql
```

- Create the ```nova_api```, ```nova```, and ```nova_cell0``` databases:
```
MariaDB [(none)]> CREATE DATABASE nova_api;
MariaDB [(none)]> CREATE DATABASE nova;
MariaDB [(none)]> CREATE DATABASE nova_cell0;

e.g.
CREATE DATABASE nova_api;
CREATE DATABASE nova;
CREATE DATABASE nova_cell0;
```

- Grant proper access to the ```glance``` database:
```
MariaDB [(none)]> GRANT ALL PRIVILEGES ON nova_api.* TO 'nova'@'localhost' IDENTIFIED BY 'NOVA_DBPASS';
MariaDB [(none)]> GRANT ALL PRIVILEGES ON nova_api.* TO 'nova'@'%' IDENTIFIED BY 'NOVA_DBPASS';

MariaDB [(none)]> GRANT ALL PRIVILEGES ON nova.* TO 'nova'@'localhost' IDENTIFIED BY 'NOVA_DBPASS';
MariaDB [(none)]> GRANT ALL PRIVILEGES ON nova.* TO 'nova'@'%' IDENTIFIED BY 'NOVA_DBPASS';

MariaDB [(none)]> GRANT ALL PRIVILEGES ON nova_cell0.* TO 'nova'@'localhost' IDENTIFIED BY 'NOVA_DBPASS';
MariaDB [(none)]> GRANT ALL PRIVILEGES ON nova_cell0.* TO 'nova'@'%' IDENTIFIED BY 'NOVA_DBPASS';

e.g.
GRANT ALL PRIVILEGES ON nova_api.* TO 'nova'@'localhost' IDENTIFIED BY 'tuckn2020';
GRANT ALL PRIVILEGES ON nova_api.* TO 'nova'@'%' IDENTIFIED BY 'tuckn2020';

GRANT ALL PRIVILEGES ON nova.* TO 'nova'@'localhost' IDENTIFIED BY 'tuckn2020';
GRANT ALL PRIVILEGES ON nova.* TO 'nova'@'%' IDENTIFIED BY 'tuckn2020';

GRANT ALL PRIVILEGES ON nova_cell0.* TO 'nova'@'localhost' IDENTIFIED BY 'tuckn2020';
GRANT ALL PRIVILEGES ON nova_cell0.* TO 'nova'@'%' IDENTIFIED BY 'tuckn2020';
```
- Exit the database access client.

- Source the admin credentials to gain access to admin-only CLI commands:
```
$ . admin-openrc
```

#### Create the service credentials:
- Create the ```nova``` user:
```
$ openstack user create --domain tuc --password-prompt nova

os@controller:~$ openstack user create --domain tuc --password-prompt nova
User Password: tuckn2020
Repeat User Password: tuckn2020
+---------------------+----------------------------------+
| Field               | Value                            |
+---------------------+----------------------------------+
| domain_id           | tuc                              |
| enabled             | True                             |
| id                  | 864e642384c94248aa458ffdc360fd1c |
| name                | nova                             |
| options             | {}                               |
| password_expires_at | None                             |
+---------------------+----------------------------------+
```
- Add the ```admin``` role to the ```nova``` user:
```
$ openstack role add --project service --user nova admin
```
- Create the ```nova``` service entity:
```
$ openstack service create --name nova --description "OpenStack Compute Service" compute

os@controller:~$ openstack service create --name nova --description "OpenStack Compute Service" compute
+-------------+----------------------------------+
| Field       | Value                            |
+-------------+----------------------------------+
| description | OpenStack Compute Service        |
| enabled     | True                             |
| id          | 9946822b1ecf4dc6bb261d776dc7eb05 |
| name        | nova                             |
| type        | compute                          |
+-------------+----------------------------------+
```

- Create the Compute API service endpoints:
```
$ openstack endpoint create --region RegionOne compute public http://controller:8774/v2.1
$ openstack endpoint create --region RegionOne compute internal http://controller:8774/v2.1
$ openstack endpoint create --region RegionOne compute admin http://controller:8774/v2.1

e.g.
openstack endpoint create --region TUCKN compute public http://10.10.0.21:8774/v2.1
openstack endpoint create --region TUCKN compute internal http://10.10.0.21:8774/v2.1
openstack endpoint create --region TUCKN compute admin http://10.10.0.21:8774/v2.1

output:
os@controller:~$ openstack endpoint create --region TUCKN compute public http://10.10.0.21:8774/v2.1
+--------------+----------------------------------+
| Field        | Value                            |
+--------------+----------------------------------+
| enabled      | True                             |
| id           | 5bc1262d9d624f59aad9d2876c99da01 |
| interface    | public                           |
| region       | TUCKN                            |
| region_id    | TUCKN                            |
| service_id   | 9946822b1ecf4dc6bb261d776dc7eb05 |
| service_name | nova                             |
| service_type | compute                          |
| url          | http://10.10.0.21:8774/v2.1      |
+--------------+----------------------------------+
os@controller:~$ openstack endpoint create --region TUCKN compute internal http://10.10.0.21:8774/v2.1
+--------------+----------------------------------+
| Field        | Value                            |
+--------------+----------------------------------+
| enabled      | True                             |
| id           | 076ea1c007304503ae082e11c3b1cffa |
| interface    | internal                         |
| region       | TUCKN                            |
| region_id    | TUCKN                            |
| service_id   | 9946822b1ecf4dc6bb261d776dc7eb05 |
| service_name | nova                             |
| service_type | compute                          |
| url          | http://10.10.0.21:8774/v2.1      |
+--------------+----------------------------------+
os@controller:~$ openstack endpoint create --region TUCKN compute admin http://10.10.0.21:8774/v2.1
+--------------+----------------------------------+
| Field        | Value                            |
+--------------+----------------------------------+
| enabled      | True                             |
| id           | ba5aa195df9240b69fcbab63569527ef |
| interface    | admin                            |
| region       | TUCKN                            |
| region_id    | TUCKN                            |
| service_id   | 9946822b1ecf4dc6bb261d776dc7eb05 |
| service_name | nova                             |
| service_type | compute                          |
| url          | http://10.10.0.21:8774/v2.1      |
+--------------+----------------------------------+
```

### Install and configure
- Install the packages:
```
# apt install nova-api nova-conductor nova-novncproxy nova-scheduler
```
- Edit the ```/etc/nova/nova.conf``` file and complete the following actions:
```bash
[api_database]
# ...
connection = mysql+pymysql://nova:tuckn2020@10.10.0.21/nova_api

[database]
# ...
connection = mysql+pymysql://nova:tuckn2020@10.10.0.21/nova

[DEFAULT]
# ...
transport_url = rabbit://openstack:tuckn2020@10.10.0.21:5672/
# ...
my_ip = 10.10.0.21

use_neutron = true
firewall_driver = nova.virt.firewall.NoopFirewallDriver

[api]
# ...
auth_strategy = keystone

[keystone_authtoken]
# ...
www_authenticate_uri = http://10.10.0.21:5000/
auth_url = http://10.10.0.21:5000/
memcached_servers = 10.10.0.21:11211
auth_type = password
project_domain_name = TUC
user_domain_name = TUC
project_name = service
username = nova
password = tuckn2020

[vnc]
enabled = true
# ...
server_listen = $my_ip
server_proxyclient_address = $my_ip
novncproxy_base_url = http://10.10.0.21:6080/vnc_auto.html

[glance]
# ...
api_servers = http://10.10.0.21:9292

[neutron]
# ...
auth_url = http://10.10.0.21:5000
auth_type = password
project_domain_name = TUC
user_domain_name = TUC
region_name = TUCKN
project_name = service
username = neutron
password = tuckn2020
service_metadata_proxy = true
metadata_proxy_shared_secret = tuckn2020


[oslo_concurrency]
# ...
lock_path = /var/lib/nova/tmp

[placement]
# ...
region_name = TUCKN
project_domain_name = TUC
project_name = service
auth_type = password
user_domain_name = TUC
auth_url = http://10.10.0.21:5000/v3
username = placement
password = tuckn2020
```

- Populate the ```nova-api``` database:
```
# su -s /bin/sh -c "nova-manage api_db sync" nova
```
- Register the ```cell0``` database:
```
# su -s /bin/sh -c "nova-manage cell_v2 map_cell0" nova
```
- Create the ```cell1``` cell:
```
# su -s /bin/sh -c "nova-manage cell_v2 create_cell --name=cell1 --verbose" nova

root@controller:~# su -s /bin/sh -c "nova-manage cell_v2 create_cell --name=cell1 --verbose" nova
b90569c8-96ac-4b2a-8c04-8d50adfe4465
```
- Populate the ```nova``` database:
```
# su -s /bin/sh -c "nova-manage db sync" nova
```
- Verify nova ```cell0``` and ```cell1``` are registered correctly:
```
# su -s /bin/sh -c "nova-manage cell_v2 list_cells" nova

root@controller:~# su -s /bin/sh -c "nova-manage cell_v2 list_cells" nova
+-------+--------------------------------------+------------------------------------------+-------------------------------------------------+----------+
|  Name |                 UUID                 |              Transport URL               |               Database Connection               | Disabled |
+-------+--------------------------------------+------------------------------------------+-------------------------------------------------+----------+
| cell0 | 00000000-0000-0000-0000-000000000000 |                  none:/                  | mysql+pymysql://nova:****@10.10.0.21/nova_cell0 |  False   |
| cell1 | b90569c8-96ac-4b2a-8c04-8d50adfe4465 | rabbit://openstack:****@10.10.0.21:5672/ |    mysql+pymysql://nova:****@10.10.0.21/nova    |  False   |
+-------+--------------------------------------+------------------------------------------+-------------------------------------------------+----------+
```
### Finalize installation
- Restart the Compute services:
```
# service nova-api restart
# service nova-scheduler restart
# service nova-conductor restart
# service nova-novncproxy restart

!copy below commands
service nova-api restart
service nova-scheduler restart
service nova-conductor restart
service nova-novncproxy restart
```

[Nova Home](../nova.md#nova-compute-service)
[Next](../compute/nova.md#install-and-configure-compute-node)