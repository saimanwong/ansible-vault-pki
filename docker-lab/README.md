# Lab environment

Spins up lab containers based on [inventory/lab/hosts.yml](../inventory/lab/hosts.yml)

## Make command

- `make` - executes `build`, `setup` and `run`
- `make build` - builds CentOS 7 base image with systemd
- `make setup` - builds CentOS 7 lab image and injects local SSH public key `${HOME}/.ssh/id_rsa.pub` into lab image
- `make run` - runs three containers based on the image produced by `make setup`
- `make ps` - lists lab containers
- `make clean` - removes lab containers
