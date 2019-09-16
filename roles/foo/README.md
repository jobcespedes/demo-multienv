Role Name
=========

Test Ansile stuff

Requirements
------------

None

Role Variables
--------------

None

Dependencies
------------

None

Example Playbook
----------------

- name: test foo_var in foo role
  hosts: "{{ this_hosts | default('localhost') }}"
  connection: local
  gather_facts: false
  roles:
    - role: foo

License
-------

Apache License, Version 2.0

Author Information
------------------

Job Cespedes: job.cespedesortiz@ucr.ac.cr
