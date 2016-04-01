ansible-beats
=============

Installs and configures a specified Beat instance.

https://www.elastic.co/guide/en/beats/filebeat/current/index.html

Requirements
------------

Ansible 2.x
Requires sudo.

Role Variables
--------------

|   Variable   | required | default    | choices | comments                                               |
|--------------|----------|------------|---------|--------------------------------------------------------|
| beat_name    |  no      | "topbeat"  |         | The name of the beat to install. The list of `supported_beats` is defined in the role vars. |
| beat_install |  no      | true       |         | A flag used to control whether the role should perform installation steps. |
| beat_version |  no      |            |         | If defined, will install the specified version. |
| beat_config  |  no      |            |         | If defined, is used to populate the beat config file. If undefined, the config file is unchanged. |


Dependencies
------------

N/A

Example Playbook
----------------

    - hosts: systems
      vars:
      roles:
         - { role: username.rolename, x: 42 }

License
-------

BSD

Author Information
------------------

Jonathan Strootman "jstroot@cyverse.org"
