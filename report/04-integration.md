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

## Test report



## Resources

1. https://galaxy.ansible.com/bertvv/dhcp/
