# Enterprise Linux Lab Report

- Student name: Ebu Bekir Tinastepe
- Github repo: https://github.com/EbuTinastepe/elnx-sme

We gaan een Samba FileServer en een FTP Server aanmaken.
De volgende rollen downloaden m.b.v. commando role-deps.
  - bertvv.samba
  - bertvv.vsftpd


## Test plan

For testing purposes, the users have a password that is identical to their name. Adapt the variables at the beginning of the script, if necessary (particularly admin_user and admin_password).

Currently, most test cases are skipped, because failing tests will probably timeout which takes a lot of time. Remove the lines with the command skip at the beginning of a test case to execute it.

Als we binnen server pr011 de commando `sudo /vagrant/test/runbats.sh` uitvoeren komen we de failures tegen die we moeten wegwerken.

## Procedure/Documentation

1. We maken de file server pr011 aan door de volgende code toe te voegen aan vagrant-host.yml file.
```Yaml
- name: pr011
  ip: 172.16.0.11
  netmask: 255.255.0.0
```
2. We maken de server aan met de commando `vagrant up`.
3. We maken een file pr011.yml aan in de map host_vars.
4. We passen site.yml aan en geven de rollen mee voor de configuratie van de server.
```Yaml
- hosts: pr011
  become: true
  roles:
    - bertvv.rh-base
    - bertvv.samba
    - bertvv.vsftpd
```
5. We installeren de rollen bertvv.samba en bertvv.vsftpd met behulp van de commando `scripts/role-deps.sh`.




## Test report

###


## Resources

1. https://galaxy.ansible.com/bertvv/samba/
2. https://galaxy.ansible.com/bertvv/vsftpd/
3. https://galaxy.ansible.com/bertvv/rh-base/
