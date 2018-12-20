# Envelope Sender Address Authorization
To ensure each mail account can only use his own aliases, we will be using our alias definition to control logins:

    smtpd_sender_login_maps = ldap:/etc/postfix/ldap/virtual_aliases.cf
