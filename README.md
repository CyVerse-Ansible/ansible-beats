ansible-beats
=============
[![Build Status](https://travis-ci.org/cyverse/ansible-beats.svg?branch=master)](https://travis-ci.org/cyverse/ansible-beats)

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
| beats_geoip  |  no      | false      |         | When true, will install the GeoLite City database. See [cyverse.geoip](https://galaxy.ansible.com/cyverse/geoip/) for more info.|


Dependencies
------------

* [cyverse.geoip](https://galaxy.ansible.com/cyverse/geoip/)

Example Playbooks
-----------------

To install `topbeat` with default configuration:

    - hosts: systems
      vars:
      roles:
         - role: cyverse.beats

To install `topbeat` with specified configuration:

    - hosts: systems
      vars:
        beat_config:
          input:
            period: 10
            procs: 
               - ".*"
            stats:
              cpu_per_core: false
              filesystem: true
              proc: true
              system: true
          output:
            elasticsearch: 
              hosts:
                - "localhost:9200" 
          logging:
            to_files: true 
            files: 
              - "/path/to/log/file.log"
          shipper:
            tags: 
              - "some_tag"
      roles:
         - role: cyverse.beats

To install `filebeat` with specified configuration:

    - hosts: systems
      vars:
        beat_name: filebeat
        beat_config:
          filebeat:
            prospectors:
              - paths:
                   - "/var/log/de/*.log"
                document_type: "de-log"
          output:
           logstash:
             hosts:
               - "{{filebeat_logstash_host}}"
             tls:
               certificate_authorities: []
             index: "{{filebeat_index_pattern}}"
          shipper:
            name: "{{ansible_hostname}}"
            tags:
                - "{{ inventory_file.split('/')[-1].split('.')[0] }}"
          logging:
            to_files: true
            level: info
            files:
              path: /var/log/filebeat
              name: filebeat
              rotateeverybytes: 10485760 
      roles:
         - role: cyverse.beats

To install `packetbeat` with GeoIP database installed and configured:

    - hosts: systems
      vars:
        beat_name: packetbeat
        beats_geoip: true
        beat_config:
          interfaces:
            device: any
          protocols:
            http:
              ports: 
                  - 80
            pgsql:  
              ports: 
                  - 5432
          output:
            elasticsearch:
              hosts:
                  - "localhost:9200"
              save_topology: true
          shipper:
            tags: 
              - some_tag
            ignore_outgoing: true
            geoip:
              paths:
                - "/usr/share/GeoIP/GeoLiteCity.dat"
          logging:
            to_files: false
            files:
              path: /var/log/packetbeat
      roles:
         - role: cyverse.beats
License
-------

BSD

Author Information
------------------

Jonathan Strootman - jstroot@cyverse.org
