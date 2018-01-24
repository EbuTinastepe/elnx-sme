# Enterprise Linux Lab Report

- Student name: Ebu Bekir Tinastepe
- Github repo: https://github.com/EbuTinastepe/elnx-sme

- Being able to set up a DHCP server for a small network
    - Understanding the configuration of DHCP and being able to adapt it for a specific situation
    - Assign basic network configuration settings to clients: IP address and subnet mask, default gateway, DNS servers
    - Setting up subnets and pools of dynamic IP addresses
    - Assign a reserved IP address based on MAC address
    - Configuring the lease time
- Being able to set up a router with VyOS
    - Configure network interfaces
    - Configure Network Address Translation
    - Set up a DNS forwarder

In this assignment, the goal is to complete the network we've been building with a DHCP server for workstations and a router. When a workstation is attached to the network, it should receive correct network settings from the DHCP server (IP address, default gateway, DNS) and be able to use the services on the local network (specifically the webserver and fileserver) and to access the Internet through the router.

## Test plan

- Create a new VirtualBox VM manually, give it two host-only network interfaces, both attached to the VirtualBox host-only network with IP 172.16.0.0/16.
- Write down the MAC address of one of the two interfaces (or set it manually).
- Boot the VM with a LiveCD ISO (e.g. Fedora, but Ubuntu, Kali, etc. should also be fine).
- Ensure both interfaces will get an IP address assigned by DHCP
- Check the IP addresses: one should be the reserved IP address, the other should come from the pool of dynamic addresses.

Finally, this VM should be able to view the "company website" by surfing to <http://www.avalon.lan> in a web browser and be able to access the fileserver both through SMB and FTP.


## Procedure/Documentation

1. Eerst installeren we de rol bertvv.dhcp met behulp van de commando `scripts/role-deps.sh`.
2. We maken de dhcp server pr001 aan door de volgende code toe te voegen aan vagrant-host.yml file.
```Yaml
- name: pr001
  ip: 172.16.0.2
  netmask: 255.255.0.0
```
3. We maken de server aan met de commando `vagrant up`.
4. We passen site.yml aan en geven de rollen mee voor de configuratie van de server.
```Yaml
- hosts: pr001
  become: true
  roles:
    - bertvv.rh-base
    - bertvv.dhcp
```
5. We voeren `vagrant provision pr001` uit zodat de rollen geïnstalleerd worden op de server.
6. We maken een bestand 'pr001.yml' aan in de map host_vars.
7. Nu gaan we de dhcp server configureren. Dit doen we in het bestand 'pr001.yml' die we reeds hebben aangemaakt. (zie bestand voor code)
8. Tenslotte maken we een Workstation aan om de DHCP Server te kunnen testen.
9. Na de DHCP Server met de workstation getest te hebben beginnen we met de configuratie van de router. We kunnen starten met het installeren van het VyOS vagrant plugin met commando `vagrant plugin install vagrant-vyos`.
10. Vervolgens openen we het bestand router-config.sh in het mapje scripts en voegen we de volgende code toe:

#### Basic settings
- Hostname = router
- Domein = avalon.lan
- SSH poort = 22
```
set system host-name 'router'
set system domain-name 'avalon.lan'
set service ssh port '22'
```

#### IP settings
- Voor WAN interface genaamd eth0 stellen we DHCP in en geven we de naam "WAN link".
- eth1 configureren we het met het ip-adres 192.0.2.254/24 en geven we de naam DMZ.
- eth2 configureren we met het ip-adres 172.16.255.254/16 en geven we de naam internal.
```
set interfaces ethernet eth0 address dhcp
set interfaces ethernet eth0 description "WAN link"
set interfaces ethernet eth1 address 192.0.2.254/24
set interfaces ethernet eth1 description DMZ
set interfaces ethernet eth2 address 172.16.255.254/16
set interfaces ethernet eth2 description internal
```

#### Network Address Translation
- NAT voor interne netwerk eth1
- NAT voor uitgaande netwerk eth0
```
set nat source rule 200 outbound-interface 'eth0'
set nat source rule 200 translation address 'masquerade'
set nat source rule 200 source address '172.16.0.0/16'
set nat source rule 300 outbound-interface 'eth1'
set nat source rule 300 translation address 'masquerade'
set nat source rule 300 source address '172.16.0.0/16'
```

#### Time
- Verwijderen van oude NTP Servers
- Toevoegen van NTP server (Belgë) met tijdzone Brussel
```
delete system ntp server 0.pool.ntp.org
delete system ntp server 1.pool.ntp.org
delete system ntp server 2.pool.ntp.org
set system ntp server 'be.pool.ntp.org'
set system time-zone Europe/Brussels
```

#### Domain Name Service
- Forwarding domain = ip-adres van de primary DNS
- Forwarding name-server = DNS van de ISP.
```
set service dns forwarding domain avalon.lan server 192.0.2.10
set service dns forwarding name-server 10.0.2.3
set service dns forwarding listen-on 'eth1'
set service dns forwarding listen-on 'eth2'
```

13. Sla nu router.config.sh op en voor de commando `vagrant provision router` uit zodat de router geconfigureerd wordt.



## Test report
- De twee interfaces krijgen hun ip-adressen zoals het hoort.
![DHCP](https://github.com/EbuTinastepe/elnx-sme/blob/master/report/img/DHCP.PNG)

- We hebben internettoegang (vanzelfsprekend)
![google](https://github.com/EbuTinastepe/elnx-sme/blob/master/report/img/google.PNG)

- Werkstation kan avalon.lan bereiken.
![avalon](https://github.com/EbuTinastepe/elnx-sme/blob/master/report/img/avalon.PNG)

- Werkstation kan browsen naar de fileserver via SMB en FTP ![smb](https://github.com/EbuTinastepe/elnx-sme/blob/master/report/img/smb.PNG)
![ftp](https://github.com/EbuTinastepe/elnx-sme/blob/master/report/img/ftp.PNG)

## Resources

- https://galaxy.ansible.com/bertvv/dhcp/
- https://wiki.vyos.net/wiki/User_Guide
- https://github.com/higebu/vagrant-vyos
- https://www.youtube.com/watch?v=mX_KIrSb6mE
- https://www.youtube.com/watch?v=S1bJpsS27Qk
