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
| Server updaten met wijzigingen               |  'vagrant provision SERVER'|
| Inloggen op server                           |  'vagrant ssh SERVER'      |
| Shell-script runnen (testen)                 |  'sudo /vagrant/test/runbats.sh' |
| Een base installeren (Ansible)               |  'ansible-galaxy install SERVER' |

## Checklist network configuration

1. Is the IP-adress correct? `ip a`
2. Is the router/default gateway correct? `ip r -n`
3. Is a DNS-server available? `cat /etc/resolv.conf`
