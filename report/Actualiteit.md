# Enterprise Linux Lab Report

- Student name: Ebu Bekir Tinastepe
- Github repo: https://github.com/EbuTinastepe/elnx-sme

- Fail2Ban configureren samen met de plugin in Wordpress.

## Test plan

- Ga naar 192.0.2.50/wordpress
- login: ebu
- paswoord: W@6@7LQX^fRIFVGaQ7
- Als dit lukt slaagt de eerste test!
- Nu gaan we meermaals met een fout wachtwoord proberen inloggen. Deze simuleren we met de commando `sudo fail2ban-client set wordpress unbanip 192.0.2.1`.
- Hierbij zou de host ip-adres geband moeten worden. Deze kunnen we unbannen met de commando `sudo fail2ban-client set wordpress unbanip 192.0.2.1`.
- Als je na het uitvoeren van de commando terug kunt inloggen betekent dat alles goed is verlopen!

## Procedure/Documentation

1. We voegen 'memiah.fail2ban-wordpress' rol toe aan site.yml bij de host pu004.
```Yaml
- hosts: pu004
  become: true
  roles:
    - bertvv.rh-base
    - bertvv.httpd
    - bertvv.mariadb
    - bertvv.wordpress
    - memiah.fail2ban-wordpress
```
2. We installeren de rol 'memiah.fail2ban-wordpress' met behulp van de commando `scripts/role-deps.sh`.
3. Open het bestand 'pr004.yml' aan in de map host_vars en voeg de volgende code toe.
```Yaml
fail2ban_wordress_plugin_name: wp-fail2ban
fail2ban_wordress_plugin_version: ""
fail2ban_wordress_plugins_dir: /usr/share/wordpress/wp-content/plugins/
fail2ban_wordress_filter_file: /filters.d/wordpress-hard.conf
fail2ban_wordress_mu_plugins_dir: false
fail2ban_wordress_jail_config:
  logpath: /var/log/messages
  maxretry: 5
  bantime: 21600
  findtime: 86400
```
4. Tot slot voeren we de commando `vagrant provision pu004` uit.

## Test report

1. Surfen naar "192.0.2.50/wordpress" en inloggen met login en paswoord. SUCCES!
![ingelogd](https://github.com/EbuTinastepe/elnx-sme/blob/master/report/img/ingelogd.PNG)
2. Na enkele verkeerde pogingen zou de server niet meer bereikbaar moeten worden.
![failure](https://github.com/EbuTinastepe/elnx-sme/blob/master/report/img/failure.PNG)
3. Server onbereikbaar na banip.
![onbereikbaar](https://github.com/EbuTinastepe/elnx-sme/blob/master/report/img/onbereikbaar.PNG)
4. Server opnieuw bereikbaar na unbanip!
![terugbereikbaar](https://github.com/EbuTinastepe/elnx-sme/blob/master/report/img/terugbereikbaar.PNG)


## Resources

- https://galaxy.ansible.com/memiah/fail2ban-wordpress/
- https://github.com/memiah/ansible-role-fail2ban-wordpress
- https://charles.lecklider.org/wordpress/wp-fail2ban
