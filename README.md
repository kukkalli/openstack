# OpenStack Installation Guide

## Initial Setup
[SSH Configuration](https://github.com/kukkalli/OpenStack/blob/master/initial-setup/ssh.md#setup-ssh-keys)

## Environment Setup
[Host Networking](https://github.com/kukkalli/OpenStack/blob/master/environment-setup/host-networking.md#host-networking)

### Network Time Protocol (NTP)
```
# apt install chrony
```
#### On Controller node:
##### edit the /etc/chrony/chrony.conf file:
```bash
# server NTP_SERVER iburst

server ntphost1.hrz.tu-chemnitz.de iburst
server ntphost2.hrz.tu-chemnitz.de iburst

allow 10.10.0.0/16
allow 10.11.0.0/16

```

##### Restart the service
```
# service chrony restart
```

##### Verify controller
```
# chronyc sources
210 Number of sources = 2
MS Name/IP address         Stratum Poll Reach LastRx Last sample
===============================================================================
^+ eris.hrz.tu-chemnitz.de       4   6    17     6  +1972us[+1917us] +/-   13ms
^* ceres.hrz.tu-chemnitz.de      3   6    17     6  -1816us[-1871us] +/-   12ms
```

#### On Other nodes:
##### edit the /etc/chrony/chrony.conf file:
```bash
server controller iburst

```

##### Restart the service
```
# service chrony restart
```

##### Verify controller
```
# chronyc sources
210 Number of sources = 1
MS Name/IP address         Stratum Poll Reach LastRx Last sample
===============================================================================
^* controller                    4   6   377    57    +11us[  +22us] +/-   11ms
```

### OpenStack packages
#### Enable the repository for Ubuntu Cloud Archive
```
# add-apt-repository cloud-archive:train
```

#### Finalize the installation
```
# apt update && apt dist-upgrade
```


#### Install the OpenStack client
```
# apt install python3-openstackclient
```

