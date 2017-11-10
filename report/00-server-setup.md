# Enterprise Linux Lab Report

- Student name: Ebu Bekir Tinastepe
- Github repo: https://github.com/EbuTinastepe/elnx-sme

1. Code ophalen uit GitHub en server installeren op VirtualBox met "vagrant up"
2. bertvv.rh-base downloaden via Ansible en installeren op de server met de "vagrant provision"
3. Testplan uitvoeren
4. Testrapport uitschrijven


## Test plan

- On the host system, go to the local working directory of the project repository
- Execute `vagrant status`
    - There should be one VM, `pu004` with status `not created`. If the VM does exist, destroy it first with `vagrant destroy -f pu004`
- Execute `vagrant up pu004`
    - The command should run without errors (exit status 0)
- Log in on the server with `vagrant ssh pu004` and run the acceptance tests. They should succeed

    ```
    [vagrant@pu004 test]$ sudo /vagrant/test/runbats.sh
    Running test /vagrant/test/common.bats
    ✓ EPEL repository should be available
    ✓ Bash-completion should have been installed
    ✓ bind-utils should have been installed
    ✓ Git should have been installed
    ✓ Nano should have been installed
    ✓ Tree should have been installed
    ✓ Vim-enhanced should have been installed
    ✓ Wget should have been installed
    ✓ Admin user bert should exist
    ✓ Custom /etc/motd should be installed

    10 tests, 0 failures
    ```

    Any tests for the LAMP stack may fail, but this is not part of the current assignment.

## Procedure/Documentatie testrapport

### Tests
In commonbats.sh de admin_user aanpassen naar "ebu".

### site.yml
Bij roles: 'bertvv.rh-base' toevoegen.

### all.yml
Administrator aanmaken met 'rhbase_admin_user' gepaard met de SSH key 'rhbase_admin_ssh_key'.

rhbase_motd op true zetten.

Bij rhbase_users wordt er gebruik gemaakt van een geëncrypteerd wachtwoord, deze is aangemaakt via mkpasswd.net. Hashtype is crypt-sha512.

De admingebruiker wordt toegevoegd aan de groepen users en wheel.

Bij rhbase_install_packages alles oplijsten wat nodig is: bash-completion, bind-utils, vim-enhanced, git, nano, tree, wget.

Uiteindelijk ziet alles er zo uit;
    ```
rhbase_motd: true
rhbase_repositories:
  - epel-release
rhbase_install_packages:
  - bash-completion
  - bind-utils
  - vim-enhanced
  - git
  - nano
  - tree
  - wget
rhbase_firewall_allow_services:
  - http
  - https
rhbase_admin_user: ebu
rhbase_admin_ssh_key: ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDPfUrrv3mde/PyDRPyCau61frmoqK//fEIW3E/mK/6iw5Hmvb6YVfqf6dKC7cPX1ksl6Mc7JiWGJfZW7ZOCq6PirGfiUiyE6lxRNuZ8xQLmPzRj5hFkqt7lqoLVGh5ldds3BMv4PRsVY2t02kjKljmqCKwk999z7yJ0mvqr922/yv28Ikt+b5CwPa4Pud/pK+Hno24rF4kAWQcYywjl5Mqmx795wy32DgeTASWy0o1VbimnWQMjsB3595eOAT9InGFEL9mNYKnVWzOKSUfmNW7KiTvtyCk0F+F91fuXID04DDfEo9C8EW5TqLAzX6VEbLeKJBSo41vcQiXSu2AvKMj ebuti@EBU-BEKIR

rhbase_users:
  - name: ebu
    comment: 'Ebu Bekir Tinastepe'
    password: $6$DDjLVsvX0vW7hFmr$RWQ5BeChdYcEefCJvhdXo53qPfKK/3lVhH7S4R67jgI5TxLULfjnTqt9Imd30YyiOdpomoCU9qg6VQgfcI.Lc0
    groups:
      - users
      - wheel
    ```


## Resources

- Syllabus voor installatie Ansible.
- https://galaxy.ansible.com/bertvv/
- https://github.com/bertvv/ansible-skeleton
- https://github.com/bertvv/ansible-role-rh-base/blob/master/README.md
- https://www.mkpasswd.net/
