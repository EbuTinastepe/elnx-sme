# Enterprise Linux Lab Report

- Student name: Ebu Bekir Tinastepe
- Github repo: https://github.com/EbuTinastepe/elnx-sme

1. Code ophalen uit GitHub en server installeren op VirtualBox met "vagrant up"
2. bertvv.rh-base downloaden via Ansible en installeren op de server met de "vagrant provision"
3. Testplan uitvoeren
4. Testrapport uitschrijven


## Test plan

Both servers can be validated with the test scripts. Execute `sudo /vagrant/test/runbats.sh` to run them. Remark that these tests run locally on the VMs. Ensure that the DNS service is also available from your host system!

We gaan er voor zorgen dat zowel de acceptatietest voor pu001 als voor pu002 slagen.
Tenslotte testen we via dig of de DNS server correct functioneert.

## Procedure/Documentation
### DNS Server

1. Eerst en vooral installeren we de role bertvv.bind. Deze hebben we nodig om een DNS server op te zetten.

2. Aan vagrant-hosts.yml voegen we deze code toe:
```
- name: pu001
  ip: 192.0.2.10
```

3. Aan site.yml voegen we deze code toe en daarna `vagrant up` en daarna `vagrant provision`:
```
- hosts: pu001
  become: true
  roles:
    - bertvv.rh-base
    - bertvv.bind
```

De role 'bind' zorgt er voor dat de server pu001 geconfigureerd wordt als een DNS server.

4. Nu moeten we de DNS server configreren:
- we maken in het mapje host_vars het bestand 'pu001.yml' aan.
- we configureren alles in het bestand 'pu001.yml' (zie bestand voor code)

5. Nadat we alles hebben geconfigureerd, voeren we de commando `vagrant provision` uit. Hiermee zal de installatie gebeuren. We kunnen nu inloggen in de server pu001 en de test runnen met commando `sudo /vagrant/test/runbats.sh`.

### Slave DNS Server

1. Aan vagrant-hosts.yml voegen we deze code toe:
```
- name: pu002
  ip: 192.0.2.11
```

2. Aan site.yml voegen we deze code toe en daarna `vagrant up` en daarna `vagrant provision`:
```
- hosts: pu002
  become: true
  roles:
    - bertvv.rh-base
    - bertvv.bind
```

3. Nu moeten we de Slave DNS server configreren:
- we maken in het mapje host_vars het bestand 'pu002.yml' aan.
- we configureren alles in het bestand 'pu002.yml' (zie bestand voor code)

4. Nadat we alles hebben geconfigureerd, voeren we de commando `vagrant provision` uit. Hiermee zal de installatie gebeuren. We kunnen nu inloggen in de server pu002 en de test runnen met commando `sudo /vagrant/test/runbats.sh`.

## Test report

### DNS Server test (pu001)
```
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
Running test /vagrant/test/pu001/masterdns.bats
 ✓ The `dig` command should be installed
 ✓ The main config file should be syntactically correct
 ✓ The forward zone file should be syntactically correct
 ✓ The reverse zone files should be syntactically correct
 ✓ The service should be running
 ✓ Forward lookups public servers
 ✓ Forward lookups private servers
 ✓ Reverse lookups public servers
 ✓ Reverse lookups private servers
 ✓ Alias lookups public servers
 ✓ Alias lookups private servers
 ✓ NS record lookup
 ✓ Mail server lookup

13 tests, 0 failures
```

### Slave DNS Server test (pu002)
```
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
Running test /vagrant/test/pu002/slavedns.bats
 ✓ The `dig` command should be installed
 ✓ The main config file should be syntactically correct
 ✓ The server should be set up as a slave
 ✓ The server should forward requests to the master server
 ✓ There should not be a forward zone file
 ✓ The service should be running
 ✓ Forward lookups public servers
 ✓ Forward lookups private servers
 ✓ Reverse lookups public servers
 ✓ Reverse lookups private servers
 ✓ Alias lookups public servers
 ✓ Alias lookups private servers
 ✓ NS record lookup
 ✓ Mail server lookup

14 tests, 0 failures
```

## Resources

- De bestanden '02.dns.md' en 'README.md'
- https://galaxy.ansible.com/bertvv/bind/
- https://galaxy.ansible.com/bertvv/rh-base/
