ansible-ssh-user-role
=========

Handles the creation of an ansible management ssh user as well as the addition of additional
ssh users.

Requirements
------------

None

Role Variables
--------------

```YAML
##
# General ssh configuration
##

# Disallow password authentication (inly use key auth)
ansible_ssh_deny_passwd_auth: false

# Deny root login - true will set without-password value
ansible_ssh_deny_root_login: false

##
# Defaults for the ansible management ssh user
##

# Name of the ansible management ssh user
ansible_ssh_mgm_user: ansible

# Example path for ansible ssh public key
ansible_ssh_mgm_user_key: "ssh-rsa AAAAB3NzaC1yc2EAAAADAQAB.......VfjQ37gTQ== Ansible Mangement User"

# Create ansible management SSH user
ansible_ssh_mgm_user_create: true

# Allow password less sudo for ansible management user
ansible_ssh_user_allow_sudo: true

# Deploy the ansible management user key
ansible_ssh_user_deploy_key: true

##
# Defaults for root setup
##

# Deploy configured root keys
ansible_ssh_deploy_root_keys: true

# List of keys for root deployment
ansible_ssh_root_keys: []

##
# Defaults for additional ssh users
##

# Create additional ssh users
ansible_ssh_create_users: true

# Deploy keys for additional ssh users
ansible_ssh_deploy_keys: true

# Additional users to be created
ansible_ssh_users: []
#    - user: username
#      keys:
#        - /path/to/key/file
#        - http://url-of-keys.list

```

Dependencies
------------

None

Example Playbook
----------------

    - hosts: servers
      roles:
         - { role: methorz.ansible_ssh_user }

Example tag usage
----------------
```YAML
# The secure ssh options require explicit tag usage to be executed
ansible-playbook secure.yml --tags "secure_ssh" 

# The ansible user setup options require explicit tag usage to be executed.
# The remote user can also be provided
ansible-playbook secure.yml -u root --tags "first_run"

```


License
-------

BSD

Author Information
------------------

https://twitter.com/methor_z
