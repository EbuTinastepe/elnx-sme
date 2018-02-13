# Evaluatie Enterprise Linux

| :---          | :---                                           |
| Student       | Ebu TINASTEPE                                  |
| Klasgroep     | gent                                           |
| Email         | <mailto:ebu.tinastepe.u0558@student.hogent.be> |
| Hoofdopdracht | SME                                            |
| Repo          | <git@github.com:EbuTinastepe/elnx-sme.git>     |

## Hoofdopdracht

W4: zit nog niet zo ver

- Opdracht 0 klaar
- Bezig met LAMP, maar nog niet veel gerealiseerd -> Tandje bijsteken!
- nog geen `host_vars/`-directory

W9:

- LAMP, DNS klaar
- Beginnen met Samba

Eindevaluatie

- Acceptatietests fileserver ok
- Werkstation:
    - IP-config: ok
    - Web: lokaal + internet ok
    - FTP via FQDN: ok
    - Samba via NetBIOS: ok
- Prima!

### Eindbeoordeling

O1: Deskundig

## Troubleshooting

### Eerste troubleshooting-opdracht

- Datalinklaag:
    - Controleer ook op *welke* HO-interface de VM is aangesloten en (op niveau Internetlaag) of de IP instellingen van die interface consistent zijn.
- Internetlaag
    - Controleer ook of je kan pingen **van hostsysteem** naar de VM! In je verslag is het niet duidelijk van waar je pingt, in demo is niet afdoende aangetoond dat dit lukt vanaf het hostsysteem
- Transportlaag:
    - Gebruik wanneer mogelijk `--add-service=` ipv `--add-port`, en nooit beide
- Applicatielaag:
    - Niet gerapporteerd
- Eindresultaat niet beschreven

Beoordeling: nog niet bekwaam

### Tweede troubleshooting-opdracht

- Kdump was geen onderdeel van de opdracht, jammer dat je hier tijd mee verloren hebt...
- Service startte niet, dus ook niet bereikbaar vanaf hostsysteem

Beoordeling: nog niet bekwaam

### Derde troubleshooting-opdracht

Ok!

### Eindbeoordeling

O2: Bekwaam

## Opdracht Actualiteit

- Fail2ban op Wordpress, verwerkt in playbook
- Tijdens de demo is de ban niet getriggerd door >5 verkeerde logins
- Manuele ban vanaf command line uitgevoerd

### Eindbeoordeling

O3: Bekwaam

## Rapportering

### Laboverslagen

In verslagen van labo's 1, 2 en 3 is bij de resultaten enkel een transcript van de acceptatietests gegeven. Daarmee is niet aangetoond dat de services ook beschikbaar zijn over het netwerk.

R1: Bekwaam

### Demonstraties

Prima.

R2: Deskundig

### Cheat sheet

- Af en toe in de loop van het semester bijgewerkt

R3: Bekwaam

