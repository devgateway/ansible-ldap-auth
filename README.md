ldap-auth
=========

Configure LDAP authentication & authorization with parametrized user/host/project lookups.

Requirements
------------

You must place your LDAP server certificate in `files/certs/*.crt`.

Role Variables
--------------

### Required variables

* `ldap_tls_cert` - TLS/SSL certificate for your LDAP server.

* `ldap_uri` - URI of your LDAP server(s). Separate multiple URIs with a space.


### Optional variables

* `ldap_authorized_keys_command_user` - the user as whom SSH daemon will run the command to fetch authorized keys from LDAP.

* `ldap_client_files` - dictionary of config templates (without `.j2` suffix) and their destinations and modes.

* `ldap_pam_param_config_file` - location of pam-param INI file.

* `ldap_pam_param_short_name` - whether to use short host names (vs FQDN) when looking up hosts in LDAP.

* `ldap_pubkey_attr` - the name of LDAP attribute that holds printable public key (possibly with comments).

* `ldap_pubkey_filter` - the filter used to look up public keys by user ID; `%s` substituted for UID.

* `ldap_sshd_config` - location of SSH daemon config.

* `ldap_sudoers` - whether to try looking up sudoers records in LDAP. If false, NSS will only consider local files. This would be useful to override generic LDAP sudoers with local settings (e.g. to disallow sudoers).

* `ldap_tls_cacertdir` - where to install LDAP certificate.

* `ldap_uid_min` - minimum numeric ID for non-system users, on most modern systems equals 1000.


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
