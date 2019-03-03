
```
apt-get install postfix postfix-mysql dovecot-core dovecot-imapd dovecot-lmtpd dovecot-mysql dovecot-pop3d
```
- https://upcloud.com/community/tutorials/secure-postfix-using-lets-encrypt/
- https://www.digitalocean.com/community/tutorials/how-to-configure-a-mail-server-using-postfix-dovecot-mysql-and-spamassassin
- https://help.ubuntu.com/community/Dovecot

```
postconf -e 'smtpd_tls_cert_file=/etc/letsencrypt/live/email.bnbpro.tn/fullchain.pem'
postconf -e 'smtpd_tls_key_file=/etc/letsencrypt/live/email.bnbpro.tn/privkey.pem'
postconf -e 'smtpd_use_tls=yes'
postconf -e 'smtpd_tls_auth_only = yes'
postconf -e 'smtpd_sasl_type = dovecot'
postconf -e 'smtpd_sasl_path = private/auth'
postconf -e 'smtpd_sasl_auth_enable = yes'
postconf -e 'smtpd_recipient_restrictions = permit_sasl_authenticated,permit_mynetworks,reject_unauth_destination'
postconf -e 'mydestination = localhost'
postconf -e 'myhostname = email.bnbpro.tn'
postconf -e 'virtual_transport = lmtp:unix:private/dovecot-lmtp'
postconf -e 'virtual_mailbox_domains = mysql:/etc/postfix/mysql-virtual-mailbox-domains.cf'
postconf -e 'virtual_mailbox_maps = mysql:/etc/postfix/mysql-virtual-mailbox-maps.cf'
postconf -e 'virtual_alias_maps = mysql:/etc/postfix/mysql-virtual-alias-maps.cf'
```
```
nano /etc/postfix/mysql-virtual-mailbox-domains.cf
		
user = root
password = Vps2019
hosts = 167.114.3.80:31378
dbname = mymail
query = SELECT 1 FROM virtual_domains WHERE name='%s'
```

```
nano /etc/postfix/mysql-virtual-mailbox-maps.cf 
		
user = root
password = Vps2019
hosts = 167.114.3.80:31378
dbname = mymail
query = SELECT 1 FROM virtual_users WHERE email='%s'
```
```
nano /etc/postfix/mysql-virtual-alias-maps.cf

user = root
password = Vps2019
hosts = 167.114.3.80:31378
dbname = mymail
query = SELECT destination FROM virtual_aliases WHERE source='%s'
````
```
mkdir -p /var/mail/vhosts/royal-immo.tn
mkdir -p /var/mail/vhosts/m.bnb.tn
mkdir -p /var/mail/vhosts/m.tangorythm.com
```
```
connect = host=167.114.3.80:31378 dbname=mymail user=root password=Vps2019
```



