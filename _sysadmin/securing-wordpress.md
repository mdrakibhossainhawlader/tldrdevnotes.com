---
layout: post
title: Securing Wordpress
ctime: 2017-07-05

---

####  File Permissions

Secured File Permissions

```
600 -rw-------  /home/user/wp-config.php
604 -rw----r--  /home/user/cgi-bin/.htaccess
600 -rw-------  /home/user/cgi-bin/php.ini
711 -rwx--x--x  /home/user/cgi-bin/php.cgi
100 ---x------  /home/user/cgi-bin/php5.cgi
```

```bash
chmod 604 .htaccess; 
chmod 600 wp-config.php;
chmod 600 php.ini cgi-bin/php.ini; 
chmod 711 cgi-bin/php.cgi;
chmod 100 cgi-bin/php5.cgi;

```

#### Limiting file access rights

Add the following to `.htaccess`

```apache
# SECURING wp-config.php
# http://codex.wordpress.org/Hardening_WordPress#Securing_wp-config.php
<files wp-config.php>
order allow,deny
deny from all
</files>

# SECURING wp-includes
# http://codex.wordpress.org/Hardening_WordPress#Securing_wp-includes
# Block the include-only files.
<IfModule mod_rewrite.c>
RewriteEngine On
RewriteBase /
RewriteRule ^wp-admin/includes/ - [F,L]
RewriteRule !^wp-includes/ - [S=3]
RewriteRule ^wp-includes/[^/]+\.php$ - [F,L]
RewriteRule ^wp-includes/js/tinymce/langs/.+\.php - [F,L]
RewriteRule ^wp-includes/theme-compat/ - [F,L]
</IfModule>

# SECURING xmlrpc.php
# https://blog.sucuri.net/2015/10/brute-force-amplification-attacks-against-wordpress-xmlrpc.html
# Prevent Brute forcing via xmlrpc.php by allowing ONLY localhost access xmlrpc.php 
# (some other plugin using xmlrpc e.g. Yoast might have an issue, check the log files, read up)
<files xmlrpc.php="">
Order Deny,Allow
Deny from all
Allow from 192.0.64.0/18
Satisfy All
ErrorDocument 403 http://127.0.0.1/
</files>
```


Links
---

- [WordPress Codex: Changing File Permissions](https://codex.wordpress.org/Changing_File_Permissions#.htaccess_permissions)