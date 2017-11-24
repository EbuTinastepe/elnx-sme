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

## Test report

The test report is a transcript of the execution of the test plan, with the actual results. Significant problems you encountered should also be mentioned here, as well as any solutions you found. The test report should clearly prove that you have met the requirements.

## Resources

- https://galaxy.ansible.com/bertvv/httpd/
- https://galaxy.ansible.com/bertvv/mariadb/
- https://galaxy.ansible.com/bertvv/wordpress/
- https://github.com/bertvv/lampstack
- https://github.com/bertvv/ansible-role-httpd
- https://github.com/bertvv/ansible-role-mariadb
- https://github.com/bertvv/ansible-role-wordpress
- https://datacenteroverlords.com/2012/03/01/creating-your-own-ssl-certificate-authority/
