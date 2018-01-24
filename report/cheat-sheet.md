# Cheat sheets and checklists

- Student name: Ebu Bekir Tinastepe
- Github repo: https://github.com/EbuTinastepe/elnx-sme

## Basic commands

| Task                | Command |
| :---                | :---    |
| Query IP-adress(es) | `ip a`  |

## Git workflow

Simple workflow for a personal project without other contributors:

| Task                                         | Command                   |
| :---                                         | :---                      |
| Current project status                       | `git status`              |
| Select files to be committed                 | `git add FILE...`         |
| Commit changes to local repository           | `git commit -m 'MESSAGE'` |
| Push local changes to remote repository      | `git push`                |
| Pull changes from remote repository to local | `git pull`                |


| Installeren van een server                   |  'vagrant up'      |
| Status vagrant                               |  'vagrant status'      |
| Herladen server                              |  'vagrant reload' |
| rsh key aanmaken                             |  'ssh-keygen -t rsa'      |
| Server updaten met wijzigingen               |  'vagrant provision HOST'|
| Inloggen op server                           |  'vagrant ssh HOST'      |
| Shell-script runnen (testen)                 |  'sudo /vagrant/test/runbats.sh' |
| Installeren van een base (Ansible)           |  'ansible-galaxy install BASE' |
| Commando als vagrant ssh niet werkt          |  'vagrant_prefer_system_bin=1 vagrant ssh HOST' |
| Script commando voor installeren van role(s) |  'scripts/role-deps.sh' |
| Installeren van een plugin met vagrant       |  'vagrant plugin install PLUGIN' |

## Checklist network configuration

1. Is the IP-adress correct? `ip a`
2. Is the router/default gateway correct? `ip r -n`
3. Is a DNS-server available? `cat /etc/resolv.conf`
