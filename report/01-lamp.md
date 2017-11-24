# Enterprise Linux Lab Report

- Student name: Ebu Bekir Tinastepe
- Github repo: https://github.com/EbuTinastepe/elnx-sme

De volgende rollen downloaden.
  - bertvv.httpd
  - bertvv.mariadb
  - bertvv.wordpress

## Test plan

## Procedure/Documentation

1. Rollen installeren: bertvv.httpd, bertvv.mariadb en bertvv.wordpress.
Hiermee wordt Apache webserver en MariaDB geïnstalleerd op de server.

2. Voeg de volgende code bij roles toe in site.yml:

```
# site.yml
---
- hosts: pu004
  become: true
  roles:
    - bertvv.rh-base
    - bertvv.httpd
    - bertvv.mariadb
    - bertvv.wordpress
```

3. Daarna moet men in het mapje host_vars een bestand pu004.yml aanmaken. En voegen we volgende code toe.

```
# puOO4.yml
rhbase_firewall_allow_services:
    - http
    - https

mariadb_databases:
  - name: wordpressdb

mariadb_users:
  - name: ebutinastepe
    password: M@ria
    priv: '*.*:ALL,GRANT'

mariadb_root_password: M@riaRoot

wordpress_database: wordpressdb
wordpress_user: ebutinastepe
wordpress_password: Wordpress
```

4. Certificaat aanmaken voor de server. Volgende commando's uitvoeren in GitBash:

```
openssl genrsa -out device.key 2048

openssl req -new -key device.key -out device.csr

openssl x509 -req -days 365 -in pu004.csr -signkey pu004.key -out pu004.crt
```

Nadat de certificaten zijn aangemaakt, deze in een folder (file-cert) stoppen in map ansible.

5. pu004.yml en site.yml aanpassen.

-pu004.yml (toevoeging)

```
httpd_SSLCertificateFile: /etc/pki/tls/certs/ca.crt
httpd_SSLCertificateKeyFile: /etc/pki/tls/private/ca.key
```
- site.yml (toevoeging)
```
  pre_tasks:
    - name: Webserver private key kopieren
      copy:
        src: certificaten/device.key
        dest: /etc/pki/tls/private/ca.key
    - name: Web server certificaat kopieren
      copy:
        src: certificaten/device.crt
        dest: /etc/pki/tls/certs/ca.crt
```

6. Als laatst passen we testfile van pu004 aan naar de zelfgegeven user en passwoorden.

```
#{{{ Variables
sut=192.0.2.50
mariadb_root_password=M@riaRoot
wordpress_database=wordpressdb
wordpress_user=ebutinastepe
wordpress_password=M@ria
```

## Test report

Na het uitvoeren van de test commando zien we dat alle testen slagen.
```
[vagrant@pu004 ~]$ sudo /vagrant/test/runbats.sh
Running test /vagrant/test/common.bats
 ✓ EPEL repository should be available
 ✓ Bash-completion should have been installed
 ✓ bind-utils should have been installed
 ✓ Git should have been installed
 ✓ Nano should have been installed
 ✓ Tree should have been installed
 ✓ Vim-enhanced should have been installed
 ✓ Wget should have been installed
 ✓ Admin user ebu should exist
 ✓ Custom /etc/motd should have been installed

10 tests, 0 failures
Running test /vagrant/test/pu004/lamp.bats
 ✓ The necessary packages should be installed
 ✓ The Apache service should be running
 ✓ The Apache service should be started at boot
 ✓ The MariaDB service should be running
 ✓ The MariaDB service should be started at boot
 ✓ The SELinux status should be ‘enforcing’
 ✓ Web traffic should pass through the firewall
 ✓ Mariadb should have a database for Wordpress
 ✓ The MariaDB user should have "write access" to the database
 ✓ The website should be accessible through HTTP
 ✓ The website should be accessible through HTTPS
 ✓ The certificate should not be the default one
 ✓ The Wordpress install page should be visible under http://192.0.2.50/wordpress/
 ✓ MariaDB should not have a test database
 ✓ MariaDB should not have anonymous users

15 tests, 0 failures


15 tests, 0 failures
```

## Resources

- https://galaxy.ansible.com/bertvv/httpd/
- https://galaxy.ansible.com/bertvv/mariadb/
- https://galaxy.ansible.com/bertvv/wordpress/
- https://github.com/bertvv/lampstack
- https://github.com/bertvv/ansible-role-httpd
- https://github.com/bertvv/ansible-role-mariadb
- https://github.com/bertvv/ansible-role-wordpress
- https://datacenteroverlords.com/2012/03/01/creating-your-own-ssl-certificate-authority/
