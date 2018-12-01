# ldap-virtualMail schema
LDAP schema for use with Postfix and Dovecot - slated for virtual users and domains.

Test the schema and create a LDIF file from it:

    mkdir ldif
    slaptest -f schema.txt -F ldif/
    config file testing succeeded

Add schema to our directory:
    
    ldapadd -ZZ -D cn=admin,cn=config -W -H ldap://ldap.example.com -f virtualMail.ldif
    adding new entry "cn=virtualMail,cn=schema,cn=config"

| Attribute         | Postfix                | Dovecot    |
|-------------------|------------------------|------------|
| mailHomeDirectory |                        | home       |
| mailAlias         | mailacceptinggeneralid |            |
| mailDrop          |                        | user       |
| mailUidNumber     |                        | uid        |
| mailGidNumber     |                        | gid        |
| mailEnabled       |                        |            |
| mailQuota         |                        | quota_rule |
| mailGroupACL      |                        | acl_groups |
| mailExpungeTrash  |                        |            |

## Dovecot LDAP scripts
Some basic LDAP maintenance scripts for [dovecot](https://github.com/tleuxner/dovecot).
