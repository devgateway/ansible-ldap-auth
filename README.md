ldap-auth
=========

Configure LDAP authentication & authorization with parametrized user/host/project lookups.

Requirements
------------

You must place your LDAP server certificate in `files/certs/*.crt`.

Role Variables
--------------

### Required variables

* `ldap_tls_cert` - 

* `ldap_uri` - 


### Optional variables

* `ldap_authorized_keys_command_user` - 

* `ldap_client_files` - 

* `ldap_pam_param_config_file` - 

* `ldap_pam_param_short_name` - 

* `ldap_pubkey_attr` - 

* `ldap_pubkey_filter` - 

* `ldap_sshd_config` - 

* `ldap_sudoers` - 

* `ldap_tls_cacertdir` - 

* `ldap_uid_min` - 


Dependencies
------------

* `local-repo` - the role adding a local repository with pam-param and getauthorizedkeys RPMs.

Example Playbook
----------------

    - hosts: ldap-auth
      roles:
        - ldap-auth
      vars:
        ldap_tls_cert: production.crt
        ldap_uri: ldaps://ldap.example.org
        ldap_sudoers: false

License
-------

GPLv3+

Author Information
------------------

Development Gateway
