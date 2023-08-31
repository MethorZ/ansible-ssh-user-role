ansible-ssh-user-role
=========

Handles the creation of an ansible management ssh user as well as the addition of additional
ssh users.

Offers a secure run in order to apply some security features to the ssh login

Requirements
------------

None

Role Variables
--------------

```YAML
##
# General ssh configuration and ansible management user setup
##

# Enable ansible ssh role
ansible_ssh_enabled: false

# Create additional ssh users
ansible_ssh_create_users: false

# Deploy keys for additional ssh users
ansible_ssh_deploy_keys: false

# SSH users to be created and have ssh keys deployed
ansible_ssh_users: []
#    # Example user
#    - user: username
#      state: present
#      keys:
#        - String
#        - http://url-of-keys.list

# Additional ssh users to be added
ansible_ssh_additional_users: []
#    # Example user
#    - user: username
#      state: present
#      keys:
#        - String
#        - http://url-of-keys.list

##
# Initial basic setup for the root and ansible management user required during
# the initial setup step
#
# Notice: The following settings are bound to the usage of the "first_run" tag!
##

# Create ansible management SSH user
ansible_ssh_mgm_user_create: false

# Name of the ansible management ssh user
ansible_ssh_mgm_user: ansible

# Allow password less sudo for ansible management user
ansible_ssh_mgm_user_allow_sudo: true

# Deploy the ansible management user key
ansible_ssh_mgm_user_deploy_key: true

# Example path for ansible ssh public key
ansible_ssh_mgm_user_key: !vault |
          $ANSIBLE_VAULT;1.1;AES256
          653531373033353762653137313...

# Creates a random root passwort, sets it and writes it to the specified password file
ansible_ssh_create_root_password: false

# File that will contain the root password after creation
ansible_ssh_root_password_file: /root/.pwd-root

##
# SSH security settings to provide higher security for the host
#
# Be careful when applying these since you may get locked out of your server!
#
# Notice: The following settings are bound to the usage of the "secure" tag!
##

# Disallow password authentication (only use key auth)
ansible_ssh_secure_deny_passwd_auth: false

# Deny root login - true will set without-password value
ansible_ssh_secure_deny_root_login: false

# Set the ssh port to use for the ssh connection
ansible_ssh_secure_port: 22
```

Dependencies
------------

None

Example Playbooks
----------------
```YAML
# Example general usage within playbook
- hosts: servers
  roles:
      - { role: methorz.ansible_ssh_user }

# Example usage of a first_run setup
- name: Initial server setup to prepare the ansible usage
  hosts: all
  user: root
  roles:
    - { role: methorz.ansible_ssh_user}
  vars:
    - ansible_ssh_enabled: true
    - ansible_ssh_mgm_user_create: true
    - ansible_ssh_mgm_user_allow_sudo: true
    - ansible_ssh_mgm_user_deploy_key: true
    - ansible_ssh_create_root_password: true

# Example usage for the secure setup
- name: Basic security topics to be applied to all hosts
  hosts: all
  roles:
    - { role: methorz.ansible_ssh_user, tags: ssh }
  vars:
    - ansible_ssh_enabled: true
    - ansible_ssh_secure_deny_passwd_auth: true
    - ansible_ssh_secure_deny_root_login: true
```
Example tag usage
----------------
```YAML
# The ansible user setup options require explicit tag usage to be executed.
ansible-playbook playbooks/firstrun.yml --ask-pass --tags "first_run" -l 1.2.3.4

# The secure ssh options require explicit tag usage to be executed
ansible-playbook playbook/security.yml --tags secure -l 1.2.3.4

```

License
-------

BSD

Author Information
------------------

https://twitter.com/methor_z
