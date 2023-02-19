# Internet-facing host setup

This repo contains a set of Ansible playbooks for preparing a host running on Public Cloud

## Configuration

Set inventory file `inventory.yml` containing the hosts to be configured.

Ip addresses and SSH remote users.


### Dependencies installation

Some ansible roles are needed.

```shell
ansible-galaxy install -r requirements.yml
```

## Backup Configuration

Hosts are configured to use Google Drive as backup repository, using [rclone](https://rclone.org/) integration in [restic](https://restic.net/).

Follow procedure indicated in [rclone documentation](https://rclone.org/drive/) to configure a Google's service account and obtain service account credentials file (JSON file). This need to be placed in `files/google_service_account.json`

rclone remote repository is configured as `gdrive`

Backup/restic configuration for each host can be specified in host var files (`host_vars`)


To configure backup on remote host execute the command:

```shell
ansible-playbook configure_backup.yml
```


### Empty Gdrive Trash commands

Deleted files are moved to gdrive trash. Maintenance tasks requires to empty trash periodically.

`rclone cleanup` command will empty gdrive trash
```shell
rclone cleanup gdrive:<path>
```

There is an open issue where emptyin trash is not working properly (https://github.com/rclone/rclone/issues/3964) and a specific command need to be executed instead. See this [rclone forum post](https://forum.rclone.org/t/empty-google-drive-trash/14331) for more details.
```shell
rclone delete gdrive: --drive-trashed-only --drive-use-trash=false --verbose=2 --fast-list
```


## Docker Installation

Docker and docker-compose can be installed for easily selfhosting applications

```shell
ansible-playbook configure_docker.yml
```