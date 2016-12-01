ansible-beats
=============
[![Build Status](https://travis-ci.org/CyVerse-Ansible/ansible-beats.svg?branch=master)](https://travis-ci.org/CyVerse-Ansible/ansible-beats)

[![Ansible Galaxy](https://img.shields.io/badge/ansible--galaxy-cyverse.beats-blue.svg)](https://galaxy.ansible.com/cyverse/beats/)

Installs and configures a specified Beat instance.

 https://www.elastic.co/guide/en/beats/filebeat/current/index.html

Requirements
------------
* Ansible 2.x

Role Variables
--------------

|   Variable       | required | default           | comments                                               |
|------------------|----------|-------------------|--------------------------------------------------------|
| beat_name        |  no      | "metricbeat"      | The name of the beat to install. The list of `supported_beats` is defined in the role vars. |
| beat_install     |  no      | true              | A flag used to control whether the role should perform installation steps. |
| beat_config      |  no      |                   | When defined, the child yaml is used to populate the beat's config file. If undefined, the config file is unchanged.`*` |
| beat_scv_state   |  no      |                   | When defined, corresponds to the desired `state` parameter of Ansible's [Service Module][ansible-service]. |
| beat_scv_enabled |  no      |                   | When defined, corresponds to the desired `enabled` parameter of Ansible's [Service Module][ansible-service].|
| beat_cfg_file    |  no      | {{beat_name}}.yml | If defined, sets the name for the config file. |
| beat_version     |  no      |                   | If defined, will install the specified version. |


`*`: You are able to use [config file namespacing][namespacing] when defining the `beat_config` variable, but it is not suggested. 

Dependencies
------------

None

Example Playbooks
-----------------

To install `metricbeat` with default configuration:

    - hosts: myhosts
      vars:
      roles:
         - role: cyverse.beats

To install `metricbeat` with specified configuration:

    - hosts: myhosts
      vars:
        beat_config:
           metricbeat.modules:
           - module: system
             metricsets:
               - cpu
               - filesystem
               - memory
               - network
               - process
             enabled: true
             period: 10s
             processes: ['.*']
             cpu_ticks: false
           - module: apache
             metricsets: ["status"]
             enabled: true
             period: 1s
             hosts: ["http://127.0.0.1"]
           output.elasticsearch:
             hosts: ["127.0.0.1:9200"]
      roles:
         - role: cyverse.beats

To install `filebeat` with specified configuration:

    - hosts: myhosts
      vars:
        beat_name: filebeat
        beat_config:
             ...
      roles:
         - role: cyverse.beats

License
-------

See LICENSE.txt

Author Information
------------------

https://cyverse.org

[ansible-service]: https://docs.ansible.com/ansible/service_module.html
[namespacing]: https://www.elastic.co/guide/en/beats/libbeat/5.0/config-file-format-namespacing.html
