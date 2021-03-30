# Ansible Vault PKI

## Run lab environment

Prerequisite
- Docker
- Ansible

[More info about docker-lab](docker-lab)

```
$ cd docker-lab
$ make all
$ cd ../
$ ansible-playbook -u root -i inventory/lab/hosts.yml vault.yml
```
