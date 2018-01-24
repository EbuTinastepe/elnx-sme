# Enterprise Linux Lab Report - Troubleshooting

- Student name: Ebu Bekir Tinastepe
- Class/group: TIN-TI-3B (Gent)

## Instructions

- Write a detailed report in the "Report" section below (in Dutch or English)
- Use correct Markdown! Use fenced code blocks for commands and their output, terminal transcripts, ...
- The different phases in the bottom-up troubleshooting process are described in their own subsections (heading of level 3, i.e. starting with `###`) with the name of the phase as title.
- Every step is described in detail:
    - describe what is being tested
    - give the command, including options and arguments, needed to execute the test, or the absolute path to the configuration file to be verified
    - give the expected output of the command or content of the configuration file (only the relevant parts are sufficient!)
    - if the actual output is different from the one expected, explain the cause and describe how you fixed this by giving the exact commands or necessary changes to configuration files
- In the section "End result", describe the final state of the service:
    - copy/paste a transcript of running the acceptance tests
    - describe the result of accessing the service from the host system
    - describe any error messages that still remain

## Report

### Vooraf
- Foutboodschap bij 'vagrant up'.
```
==> server: >>> Applying firewall rules
==> server: success
==> server: Error: INVALID_SERVICE: 67/tcp
```
- Open map 'provisioning' en pas het bestand 'server.sh' aan. Verander code `firewall-cmd --permanent --add-service 67/tcp` naar `firewall-cmd --permanent --add-service dhcp`.
- Kabels aangesloten? Router OK! Server OK! Workstation netwerkadapter 1 staat aangesloten, deze mag niet aangesloten worden, vinkje wegdoen.
- Toestenbord 'be' instellen met commando:
Workstation en Server: `sudo localectl set-keymap be`.
Router: `sudo loadkeys fr`.

### Link layer
- Commando "ip l" of "ip link" uitvoeren.
- Kijken of dat de interfaces UP zijn -> UP!

### Internet Layer

#### Router
- Commando `ip a` OF `ip address` uitvoeren.
- We verwachten het IP-adres "10.0.2.15/24" voor enp0s3 (NAT interface) en dat blijkt zo te zijn.
- We verwachten het IP-adres "x.x.x.2/24" voor enp0s8, maar we zien hier /16, dus veranderen we deze naar "172.20.0.254/24".
- Default gateway bekijken: commando `ip r` OF `ip route` uitvoeren.
- We verwachten "10.0.2.2" en dat blijkt zo te zijn.
- DNS-server bekijken: commando `cat /etc/resolv.conf` uitvoeren.
- We verwachten "10.0.2.3" en dat blijkt zo te zijn.

#### Server
- Commando `ip a` OF `ip address` uitvoeren.
- We verwachten het IP-adres "10.0.2.15" voor enp0s3 (NAT interface) en dat blijkt zo te zijn.
- We verwachten het IP-adres "x.x.x.2/24" voor enp0s8, maar we zien hier "172.20.0.101/16" terug.
- Dus voeren we de commando `vi /etc/sysconfig/network-scripts/ifcfg-enp0s8`. Hier corrigeren we het ip-adres naar "172.20.0.2/24".
- Vervolgens voeren we de commando's `sudo ifdown enp0s8` en daarna `sudo ifup enp0s8` uit.
- Default gateway bekijken: commando `ip r` OF `ip route` uitvoeren.
- We verwachten "10.0.2.2" en dat blijkt zo te zijn.
- DNS-server bekijken: commando "cat /etc/resolv.conf" uitvoeren.
- We verwachten "10.0.2.3" en dat blijkt zo te zijn.

#### Workstation
- Commando `ip a` OF `ip address` uitvoeren.
- geen ip adres -> fout dhcp?
- Commando `ip r` OF `ip route` uitvoeren.
- Geen DG -> fout dhcp?

- Geen ip-adres & geen default-gateway = probleem met dhcp!

### Transport & Applicatie Layer
- dhcp status checken met commando `sudo systemctl status dhcpd` -> inactive!
- dhcpd was uitgeschakeld en moest opgestart worden met commando `sudo systemctl start dhcpd`.

- dns status checken met commando `sudo systemctl status dnsmasq` -> active!
- nameserver file controleren met commando `sudo vi /etc/hots`.
- ip-adres van server verkeerd! Wijzigen van "172.20.0.101" naar "172.20.0.2".
- dns herstarten met commando `sudo systemctl restart dnsmasq`.

- apache status checken met commando `sudo systemctl status httpd` -> active!

- Firewalls gecontroleerd met commando `sudo firewall-cmd --list-all`, alleen dns niet toegevoegd.
- Toevoegen met commando `sudo firewall-cmd --add-service=dns --permanent`
- Daarna firewall herstarten met commando `sudo firewall-cmd --reload`.

- Tenslotte rebooten met commando `reboot` om te controleren of alle services automatisch starten.
- Nog eens de status checken van alle voorgenoemde services. apache was nog niet actief, werd niet automatisch geactiveerd.
- apache activeren met commando `sudo systemctl enable httpd`.
- Nog eens rebooten en opnieuw de services controleren. -> Alles actief!


## End result
Alle testen zijn geslaagd!
Uitgevoerd met commando `/usr/local/bin/acceptance-tests`
![Testuitslag troubleshoot](https://github.com/EbuTinastepe/elnx-sme/blob/master/report/img/testuitslag.PNG)

## Resources

- https://wiki.archlinux.org/index.php/Keyboard_configuration_in_Xorg
- http://www.tux-planet.fr/basculer-un-clavier-en-azerty-ou-en-qwerty-sous-linux/
- https://stackoverflow.com/questions/33029708/how-to-bring-bond0-eth0-interface-up
- https://www.tecmint.com/ifconfig-command-examples/
- https://bertvv.github.io/presentation-el7-basics/
- https://bertvv.github.io/linux-network-troubleshooting/network-access-layer.html
- https://www.centos.org/docs/5/html/5.1/Deployment_Guide/s1-networkscripts-files.html
- PDF NetworkTroubleshooting.pdf
