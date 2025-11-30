# Ansible Role: ans-compose


## Description

Deploy defined docker compose project to the server.

## Requirements

- **Ansible >= 2.7**
- Installed **docker** on the server running under **group docker**
- Installed **docker-compose** on the server

## Role Variables

All variables which can be overridden are stored in [defaults/main.yml](defaults/main.yml) and are listed in the table below.


| Name           | Default Value | Description                        |
| -------------- | ------------- | -----------------------------------|
| `project_user` | "" | Runtime user |
| `project_dirs` | [] | Prepare directory structure under /home/{{project_dir}}/ |
| `project_envs` | [] | Configuration for .env (it is optional) |
| `project_compose` | "" | Compose file content |


## Example

### Playbook

Use it in a playbook as follows:

```yaml
- host: app-server
  roles:
  - role: ans-compose
    tags:
    - compose
  vars:
    project_user: example
    project_dirs:
    - path: data
      mode: "0750"
      owner: example
    project_envs:
    # should be in vault
    - "ROOT_PASSWORD: Special-Password.123"
    project_compose: |
      version: '3.1'
    
      services:
    
        adminer:
          image: adminer
          restart: always
          ports:
            - 8080:8080
    
        db:
          image: mysql:5.6
          restart: always
          environment:
            MYSQL_ROOT_PASSWORD: $ROOT_PASSWORD
          volumes:
          - /var/lib/mysql:/home/example/data

```
