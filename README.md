# Sensu deploying role
This is a fork of an official Sensu deploying ansible role. \
This fork solves the errors, happened during the usage of unarchive-ansible-module for a remote tar.gz file

Ansible Version: 2.5 \
Ubuntu 16.04 and Cent OS 7

```
[Errno 13] Permission denied: '/var/tmp/ansible-tmp...
```


## Official Readme
Check the official readme for the details:
https://github.com/sensu/sensu-ansible/blob/master/README.md



## Playbook Prerequirements

To install Sensu with this fork of the role - several prerequirements must be met.


### Role installation

The playbook requires this fork of the ansible role installed. \
The installation of the fork globally can be done with:

```
mkdir -p /etc/ansible/roles
git clone https://github.com/skipidar/sensu-ansible.git /etc/ansible/roles/cmacrae.sensu/
```

### Ansible inventory 
Some groups in the ansible inventory - a required. /
The following ansible-hosts file will make sensu be installed locally.

Achtung: \
localhost is not accepted by redis. It requires the ip instead.

```

[sensu_masters]
127.0.0.1   ansible_connection=local

[redis_servers]
127.0.0.1   ansible_connection=local

[rabbitmq_servers]
127.0.0.1   ansible_connection=local
			
```




## Ansible PlayBook

The playbook, which installs  Sensu Master Sensu Master with all dependencies:
 - RabbitMQ
 - Redis
 - Sensu API
 - Sensu Master
 - Sensu Client (on the Master)


```
---
- name: Install sensu
  hosts: sensu_masters
  become: true
  roles:
    - role: cmacrae.sensu
      rabbitmq_server: true
      redis_server: true
      sensu_master: true
      sensu_include_dashboard: true         
      uchiwa_users: [{username: user, password: password}]
      uchiwa_port: 3000
      
```

## Role Variables
Additional Role variables

| Variable        | Describtion           | Example  |
|:------------- |:-------------|:----- |
| sensu_client_networkadapter_name      | The name of teh network adapter, which should be used, to retrieve the ipv4, visible in the sensu client | ansible_enp0s8 |


## Test with Vagrant
Use Vagrant to execute a deployment locally. /
Check the vagrant project [here](https://github.com/skipidar/sensu-ansible/tree/master/tests)

