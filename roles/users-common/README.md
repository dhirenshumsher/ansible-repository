
MIP Ansible Users Management- users-common
==================================

**Ansible role used to manage users, groups, alias, ssh authorized keys, sudo and user deprecation.**


Requirements
------------

This role was developed and tested on Ansible 2.4.1 or higher.

Role Variables
--------------

List of variables and default values:

# The default shell used for all users, when left unspecified
users_default_shell: /bin/bash

# The default action when primary_group is defined but the group does not exist
users_create_primary_group: true

# If the users role should support sudo or not (see sudo.yml for tasks)
users_enable_sudo: true

# Default sudo package name, which seems identical across distributions
users_sudo_pkg_name: sudo

# Enable to create user home directory
users_create_homedirs: true


    # The only mandatory parameter is the name.
    users:
      - name: ''                           # The username of the user.
        fullname: ''                       # The comment field, also known and used for the real name of the user.
        homedir: ''                        # The home directory of the user.
        primary_group: ''                  # The primary user group.
        groups: []                         # A list of complementary groups for the user.
        primary_gid:                       # Primary Group ID
        profile:                           # User aliases on .bash_profile
        no_create_home: false              # If true, do not create a home directory. Defaults to true if `system: true`.
        shell: "{{ users_default_shell }}" # The default user shell.
        ssh_authorized_keys: []            # A list of ssh public keys to add to add to an authorized_keys file.
        sudo: ''                           # The sudo string that will be used to configure sudo.
        system: false                      # If true, the user will be a system user. This does not affect existing users.

Dependencies
------------

None

Example Playbook
----------------

Add or modify a user and set up sudo and ssh authorized keys:

roles:
  - role: users-common
    users_default_shell: /bin/bash
    users_create_primary_group: true
    users_enable_sudo: true
    users_sudo_pkg_name: sudo
    users_create_homedirs: true
    users:
       - name: ansible
         fullname: MIP ansible user
         primary_group: ansible
         groups: ['ansible','wheel']
         primary_gid: 52000
         uid: 52000000
         homedir: /home/ansible
         profile: |
           alias ll='ls -lah'
         ssh_authorized_keys:
             - "ssh-rsa AAAA---BBB ACB@OSTML022222"
             - "ssh-rsa AAAA---CCC BAC@OSTML023333"
         sudo: 'ALL=(ALL) NOPASSWD: ALL'
         system: false

         - name: bnsible
          fullname: BIP ansible user
          primary_group: bnsible
          groups: ['bnsible','wheel']
          primary_gid: 62000
          uid: 62000000
          homedir: /home/bnsible
          profile: |
            alias ll='ls -lah'
          ssh_authorized_keys:
              - "ssh-rsa AAAA---BBB ACB@OSTML022222"
              - "ssh-rsa AAAA---CCC BAC@OSTML023333"
          sudo: 'ALL=(ALL) NOPASSWD: ALL'
          system: false


Deleting users:

    - hosts: all
      roles:
        - role: users-common
          users_deleted:
            - name: ansible
            - name: bnsible

License
-------
APACHE 2.0

Author Information
------------------
dhirenshumsher
