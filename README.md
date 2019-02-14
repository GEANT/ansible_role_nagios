ansible_role_nagios
===================


Deploy nagios on Ubuntu 16.04.
This role aims to give maxium flexibiltiy in configuring nagios.
The services/hostgroups/etc can be fully templated.
The nagios configuration parameters can be set in a dictionary.

This role only deploys nagios.
You also needed:

* some method of relaying mail
* a web server

Example Playbook
----------------


    - hosts: nagios
      become: yes
      tasks:
        - import_role:
            name: GEANT/ansible_role_nagios
      vars:
        nagios_privkey: "{{ lookup('file', '/path/to/id_rsa') }}"
        nagios_pubkeys:
          - "{{ lookup('file'), '/path/to/id_rsa.pub') }}"
        nagios_users:
          - user1
          - user2
        nagios_admins:
          - user1
        nagios_cfg:
          cgi:
            url_html_path: /
            escape_html_tags: 0
            result_limit: 0
            authorized_for_system_information: "{{ nagios_admins | join(',') }}"
            authorized_for_configuration_information: "{{ nagios_admins | join(',') }}"
            authorized_for_system_commands: "{{ nagios_admins | join(',') }}"
            authorized_for_all_services: "{{ nagios_users | join(',') }}"
            authorized_for_all_hosts: "{{ nagios_users | join(',') }}"
            authorized_for_all_service_commands: "{{ nagios_admins | join(',') }}"
            authorized_for_all_host_commands: "{{ nagios_admins | join(',') }}"
          nagios:
            check_external_commands: 1
            command_check_interval: -1
            service_check_timeout: 360
          # Pull in git repositories, for use in service checks
          nagios_checks_repos:
            - repo: https://github.com/dnmvisser/nagios-hsts-check.git
              dest: nagios-hsts-check

Then provide a template `nagios_localconf.j2` and `nagios_sshconf.j2` where you define all hosts, host_groups, contact, services, etc.


License
-------

GNU General Public License v3.0

Author Information
------------------

Dick Visser, dick.visser@geant.org
