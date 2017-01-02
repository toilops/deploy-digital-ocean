deploy-digital-ocean
=========
[![Build Status](https://travis-ci.org/BondAnthony/deploy-digital-ocean.svg?branch=master)](https://travis-ci.org/BondAnthony/deploy-digital-ocean)

Create droplets within the Digital Ocean ecosystem. Tag all droplets to ansible managed tag, return the ip address and droplet ID. 

Requirements
------------

This role does require you to install the `dopy` python module.
```
pip install -r requirements.txt
```

SSH key needs to be set within your Digital Ocean account before using this role. You will need to pass your Digital Ocean ssh key id to the role. To find your ssh key id you can make a simple GET request to the `/v2/account/keys` api. API docs can be found [here](https://developers.digitalocean.com/documentation/v2/#list-all-keys) 

Role Variables
--------------

We require the following variables to be set when including the role. I would suggest setting these variables within a vault file inside your host_vars directory.
```yaml
do_ssh_key_id: number
```
and
```yaml
do_api_key: string
```

The below variables are set within the defaults directory. These can be overridden if you need to deploy a bigger droplet or change your droplet name.. 
```yaml
do_api_key: "{{ vault_do_api_key }}"
do_ssh_key_id: "{{ vault_do_ssh_key_id }}"
droplet_name: dev0ansible
droplet_size: 512mb
do_region: nyc3
droplet_image_id: centos-6-x64
do_tag_name: ansible_mng
```

Example Playbook
----------------

Simple role include example:
```yaml
    - hosts: servers
      roles:
         - { role: BondAnthony.deploy-digital-ocean, droplet_name: dev2ansible }
```
License
-------

BSD

Author Information
------------------

[Find me on github @BondAnthony](https://github.com/BondAnthony)
