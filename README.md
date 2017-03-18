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
The role will handle creating the ssh key if the `do_ssh_key_id` is undefined. When using an existing ssh key already within your Digital Ocean account you will need to retrieve the key id and set the variable. To find your ssh key id you can make a simple GET request to the `/v2/account/keys` api. API docs can be found [here](https://developers.digitalocean.com/documentation/v2/#list-all-keys) 

Role Variables
--------------

We require the following variables to be set when including the role. I would suggest setting these variables within a vault file inside your host_vars directory for the endpoint that will be running this play.
```yaml
do_ssh_key_id: number
```
and
```yaml
do_api_key: string
```

The role will also load a public ssh key into your Digital Ocean account the first time it runs on this host. When the key fingerprint matches another key the role will fail. By default the role will look in `~/.ssh/id_rsa.pub`, this value can be overridden. The key name in your DO account will be`ansible_crtl_hostname` of the server this role was executed on.
```yaml
local_pub_key: "{{ lookup('file', '~/.ssh/id_rsa.pub') }}"
```

The below variables are set within the defaults directory. These can be overridden if you need to deploy a bigger droplet or change your droplet name.. 
```yaml
do_api_key: "{{ vault_do_api_key }}"
do_ssh_key_id: "{{ vault_do_ssh_key_id }}"
do_ssh_key_name: "ansible_crtl_{{ ansible_hostname }}"
droplet_name: 
    - dev0ansible
droplet_size: 512mb
do_region: nyc3
droplet_image_id: centos-6-x64
do_tag_name: ansible_mng
```
The `do_ssh_key_name` is used when adding your localhost ssh key to your Digital Ocean account. This will only run when `do_ssh_key_id` is undefined. When the role executes it will add the ssh key if the key finger print isn't found within your account. For each subsequent run the `ssh_key_id` will be used. This basically allows you to create an `ansible_crtl_hostname` key and use that key going forward.

The `droplet_name` is a list and will default to a single host called `dev0ansible`. You can increase the number of hosts by using the `--extra-vars` argument and building a list `--extra-vars='{"droplet_name":["dev1ansible","dev2ansible"]}'`

Example Playbook
----------------

Simple role include example:
```yaml
    - hosts: servers
      roles:
         - { role: BondAnthony.deploy-digital-ocean, droplet_name: ["dev2ansible","dev3ansible"] }
```
License
-------

BSD

Author Information
------------------

[Find me on github @BondAnthony](https://github.com/BondAnthony)
