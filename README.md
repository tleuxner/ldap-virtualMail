# ldap-virtualMail schema
LDAP schema for use with Postfix and Dovecot - slated for virtual users and domains.

    1.3.6.1.4.1.53373.2             LDAP Branch - Branch Start
    1.3.6.1.4.1.53373.2.1           LDAP AttributeTypes
    1.3.6.1.4.1.53373.2.2           LDAP ObjectClasses

Test the schema and create a LDIF file from it:

    mkdir ldif
    slaptest -f schema.txt -F ldif/
    config file testing succeeded

Add schema to our directory:
    
    ldapadd -ZZ -D cn=admin,cn=config -W -H ldap://ldap.example.com -f virtualMail.ldif
    adding new entry "cn=virtualMail,cn=schema,cn=config"

| Attribute         | Postfix                | Dovecot     |
|-------------------|------------------------|-------------|
| mailHomeDirectory |                        | home        |
| mailAlias         | mailacceptinggeneralid |             |
| mailDrop          | maildrop               | user        |
| mailUidNumber     |                        | uid         |
| mailGidNumber     |                        | gid         |
| mailEnabled       |                        |             |
| mailQuota         |                        | quota_rule  |
| mailGroupACL      |                        | acl_groups  |
| mailExpungeTrash  |                        | autoexpunge |
| mailForward       |                        |             |

Some query examples using the schema for [Postfix and Dovecot](https://github.com/tleuxner/ldap-virtualMail/tree/master/etc).

## Dovecot LDAP scripts
Some basic LDAP maintenance scripts for [dovecot](https://github.com/tleuxner/dovecot).

## Real life ACL example
Restricted accounts will be able to modify user attributes or the user password only.

```
olcAccess: {0}to dn.subtree="ou=Users,ou=Mail,dc=example,dc=com" by dn.exact
 ="cn=mailAccounts,ou=Admins,ou=Mail,dc=example,dc=com" write by * break
olcAccess: {1}to dn.subtree="ou=Users,ou=Mail,dc=example,dc=com" attrs=userP
 assword,shadowLastChange by self write by dn.children="ou=Admins,ou=Mail,dc
 =example,dc=com" write by anonymous auth by * none
olcAccess: {2}to attrs=userPassword,shadowLastChange by self write by anonym
 ous auth by * none
olcAccess: {3}to dn.base="" by users read
olcAccess: {4}to * by users read
```
