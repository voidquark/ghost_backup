# Ansible Role - Ghost Backup

[![License](https://img.shields.io/github/license/voidquark/ghost_backup)](LICENSE)

Ghost backup role for https://ghost.org blog platform. This role expected that you have already some central ansible server from where this role is executed. Backup is not stored on ghost(target) server but rather on ansible central server.

 Features of this role:
- create MySQL dump ( by default with utf8mb4 encoding ) with GZ compression
- archive ghost `content` directory with ZIP compression
- Copy Database and Content backup to central ansible server
- Prune old local backups ( by default 30days )

## Requirements

Ensure that your ansible host have the following collection installed:
- `ansible-galaxy collection install community.mysql`
- `ansible-galaxy collection install community.general`

Role is using `json_query` [filter](https://docs.ansible.com/ansible/latest/user_guide/playbooks_filters.html#selecting-json-data-json-queries). For that reason is required to install python `jmespath` on your ansible host ( ansible controller ).

## Role Variables

- **Default Variables**. Usually there is no need to change this but rather overwrite value in `host_vars` or `group_vars` if required.

| Variable Name  | Default Value | Description
| ----------- | ----------- | ----------- |
| `ghost_backup_db_encoding` | `"utf8mb4"` | Encoding mode to use for MySQL dump.
| `ghost_backup_remove_older_files` | `"30d"` | Remove local backup older than 30days.
| `ghost_backup_content_path` | `"{{ ghost_backup_root_dir }}/content"` | Ghost website content path.
| `ghost_backup_production_config_file` | `"{{ ghost_backup_root_dir }}/config.production.json"` | Ghost `config.production.json`. Required for role to extract DB name, DB Username, DB Password.

- You must define at least the following variables in `host_vars` or `group_vars`

| Variable Name  | Example Value | Description
| ----------- | ----------- | ----------- |
| `ghost_backup_local_dir` | `"/backups/ghost/example_com/"` | Destination on localmachine where backup should be stored. By default older backup than 30 days is removed. This path must exist before you run this role !
| `ghost_backup_root_dir` | `"/var/www/example"` | Root directory where your ghost app is installed.

## Dependencies

- No Dependencies

## Example Playbook

How you can run this role:

- Create the following playbook.
```yaml
- name: Execute backup for ghost website
  hosts: your_host
  gather_subset: "date_time"
  become: true
  roles:
    - ghost_backup
```
## License

MIT
## Author Information

Created by [VoidQuark](https://voidquark.com)
