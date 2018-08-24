OpenCIT Anisble
===============

Ansible role to deploy opencit.

Roles to deploy needed packages, clone relevant repos and perform ant / maven
build and finally install OpenCIT.

Role Variables
--------------

    tecris.maven
    maven_major: 3
    maven_version: 3.5.4
    maven_home_parent_directory: /opt

Example Playbook
----------------
    ---
    - hosts: all
      remote_user: root
      roles:
        - geerlingguy.repo-epel
        - { role: tecris.maven, maven_major: 3, maven_version: 3.5.4, maven_home_parent_directory: /opt, become: yes }
        - openci

License
-------

Apache 2.0

Author Information
------------------

Luke Hinds (lhinds@redhat.com)
# ansible-tpm-simulator
