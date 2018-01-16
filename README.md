ldap-auth
=========

Configure LDAP authentication & authorization with parametrized user/host/project lookups.

Requirements
------------

You must place your LDAP server certificate in `files/certs/*.crt`.

Role Variables
--------------

### General LDAP configuration

* `ldap_uri` - URI of your LDAP server(s). Separate multiple URIs with a space.

* `ldap_dn`, `ldap_pw` *(optional)* - credentials to connect to your LDAP server with.

### LDAP lookup configuration for NSS and pam-param

* `ldap_base_sudoers` - base for sudoers records lookups.

* `ldap_lookups` - a dictionary. Key `hosts` describes SSH hosts, `user` describes all user accounts, `membership` cross-references user and host DNs for access control, and `admin` looks up user accounts which get unconditional access and bypass membership check. Each item is a dictionary with three keys: `base`, `scope` (defaulting to *sub*), and `filter`.

* `ldap_nss_user_filter` - filter for NSS user lookups. NSS will add user ID clause to this filter.

### Optional variables

* `ldap_authorized_keys_command_user` - the user as whom SSH daemon will run the command to fetch authorized keys from LDAP.

* `ldap_base` - default LDAP base for libldap.

* `ldap_client_files` - dictionary of config templates (without `.j2` suffix) and their destinations and modes.

* `ldap_netgroup_base`, `ldap_netgroup_scope`, `ldap_netgroup_filter` - netgroup lookup parameters.

* `ldap_pam_param_config_file` - location of pam-param INI file.

* `ldap_pam_param_short_name` - whether to use short host names (vs FQDN) when looking up hosts in LDAP.

* `ldap_pubkey_attr` - the name of LDAP attribute that holds printable public key (possibly with comments).

* `ldap_pubkey_filter` - the filter used to look up public keys by user ID; `%s` substituted for UID.

* `ldap_sshd_config` - location of SSH daemon config.

* `ldap_sudoers` - whether to try looking up sudoers records in LDAP. If false, NSS will only consider local files. This would be useful to override generic LDAP sudoers with local settings (e.g. to disallow sudoers).

* `ldap_uid_min` - minimum numeric ID for non-system users, on most modern systems equals 1000.

Example Playbook
----------------

    - hosts: ldap-auth
      roles:
        - ldap-auth
      vars:
        ldap_uri: ldaps://ldap.example.org
        ldap_sudoers: false
        ldap_base: dc=example,dc=org
        ldap_base_sudoers: ou=sudoers,{{ ldap_base }}
        ldap_lookups:
          admin:
            base: cn=admins,ou=groups,{{ ldap_base }}
            scope: base
            filter: (member=%s)
          user:
            base: ou=people,{{ ldap_base }}
            scope: sub
            filter: (&(objectClass=posixAccount)(uid=%s)(gidNumber=100))
          host:
            base: ou=hosts,{{ ldap_base }}
            scope: sub
            filter: (&(|(objectClass=virtualMachine)(objectClass=server))(cn=%s))
          membership:
            base: ou=projects,{{ ldap_base }}
            scope: sub
            filter: (&(objectClass=groupOfNames)(member=%1$s)(serverMember=%2$s))

License
-------

GPLv3+

Author Information
------------------

Development Gateway
