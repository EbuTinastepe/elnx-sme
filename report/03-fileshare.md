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
3. We passen site.yml aan en geven de rollen mee voor de configuratie van de server.
```Yaml
- hosts: pr011
  become: true
  roles:
    - bertvv.rh-base
    - bertvv.samba
    - bertvv.vsftpd
```
4. We installeren de rollen bertvv.samba en bertvv.vsftpd met behulp van de commando `scripts/role-deps.sh`.
5. We voeren `vagrant provision pr011` uit zodat de rollen geïnstalleerd worden op de server.
6. We maken een bestand 'pr011.yml' aan in de map host_vars.
7. Nu gaan we de samba fileserver configureren. Dit doen we in het bestand 'pr011.yml' die we reeds hebben aangemaakt. (zie bestand voor code)
8. Nadat we alles van samba hebben geconfigureerd, voeren we de commando `vagrant provision` uit. Hiermee zal de configuratie uitgevoerd worden.
9. Nu moeten we de FTP server nog configureren. We voegen deze code toe in het bestand 'pr011.yml':
```Yaml
vsftpd_anonymous_enable: false
vsftpd_listen: true
vsftpd_local_enable: true
```
10. Tot slot configureren we de permissies voor sales en IT met behulp van ACL die we integreren in het bestand 'site.yml' onder host pr011. (zie bestand voor code)
11. Nadat we alles van samba hebben geconfigureerd, voeren we de commando `vagrant provision` uit. Hiermee zal de configuratie uitgevoerd worden. We kunnen nu inloggen op de server pr011 en de test runnen met commando `sudo /vagrant/test/runbats.sh`.

## Test report
```
[vagrant@pr011 ~]$ sudo /vagrant/test/runbats.sh
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
Running test /vagrant/test/pr011/samba.bats
 ✓ The ’nmblookup’ command should be installed
 ✓ The ’smbclient’ command should be installed
 ✓ The Samba service should be running
 ✓ The Samba service should be enabled at boot
 ✓ The WinBind service should be running
 ✓ The WinBind service should be enabled at boot
 ✓ The SELinux status should be ‘enforcing’
 ✓ Samba traffic should pass through the firewall
 ✓ Check existence of users
 ✓ Checks shell access of users
 ✓ Samba configuration should be syntactically correct
 ✓ NetBIOS name resolution should work
 ✓ read access for share ‘public’
 ✓ write access for share ‘public’
 ✓ read access for share ‘management’
 ✓ write access for share ‘management’
 ✓ read access for share ‘technical’
 ✓ write access for share ‘technical’
 ✓ read access for share ‘sales’
 ✓ write access for share ‘sales’
 ✓ read access for share ‘it’
 ✓ write access for share ‘it’

22 tests, 0 failures
Running test /vagrant/test/pr011/vsftp.bats
 ✓ VSFTPD service should be running
 ✓ VSFTPD service should be enabled at boot
 ✓ The ’curl’ command should be installed
 ✓ The SELinux status should be ‘enforcing’
 ✓ FTP traffic should pass through the firewall
 ✓ VSFTPD configuration should be syntactically correct
 ✓ Anonymous user should not be able to see shares
 ✓ read access for share ‘public’
 ✓ write access for share ‘public’
 ✓ read access for share ‘management’
 ✓ write access for share ‘management’
 ✓ read access for share ‘technical’
 ✓ write access for share ‘technical’
 ✓ read access for share ‘sales’
 ✓ write access for share ‘sales’
 ✓ read access for share ‘it’
 ✓ write access for share ‘it’

17 tests, 0 failures
```


## Resources

1. https://galaxy.ansible.com/bertvv/samba/
2. https://galaxy.ansible.com/bertvv/vsftpd/
3. https://galaxy.ansible.com/bertvv/rh-base/
4. https://www.mkpasswd.net/index.php
5. https://www.samba.org/samba/docs/man/manpages-3/smb.conf.5.html
6. http://permissions-calculator.org
