# Ansible Role: pg-failover 

Simple role to failover PostgreSQL servers in a replication configuration.

## Requirements

Postgresql will need to be installed on all the systems and replication should be configured; note that this role requires postgres user and root access, so either run it in a playbook with a global `become: yes`, or invoke the role in your playbook like:

    - hosts: database
      var_file: 
        - /path/to/vars/file
      roles:
        - role: ansible-role-pg-failover
          become: yes

## Role Variables

Available variables are listed below, along with default values (see `defaults/main.yml`):

Below is a list of all basic variables used throughout the role.

| Variable | Description | Required | Defaults |
|:--------:|:-----------:|:--------:|:--------:|
| **pg_replication_master** | The name of the Postgresql server that will be the primary database server. This will be the same name as the inventory name. | yes | Not set | 	 
| **pg_replication_db** | The database that will be replicated | yes | postgres |
| **pg_replication_database_user** | Which local user is used to manage the postgresql services, files ect.. | yes | postgres |
| **pg_replication_database_group** | Which local group is used to manage the postgresql services, files ect.. | yes | postgres |
| **pg_replication_type** | How replication will be done. Options are logical and log_shipping_with_streaming | yes | log_shipping_with_streaming |
| **pg_replication_postgresql_path** | Where postgresql server files have been installed | yes | /var/lib/pgsql |
| **pg_replication_postgresql_data_dir** | What the name of the postgresql server data directory is. | yes | data |
| **pg_replication_export_path** | Location of export files will be dumped. | yes | /tmp/ |
| **pg_replication_local_export_path** | Location of where remote export files will be dumped when copied to ansible executing host. | yes | /tmp/ |
| **pg_replication_export_file** | Name of the export file that will be created if the database is backed up. | yes | postgresql_data.tar.gz |
| **pg_replication_trigger_file** | Path to where the trigger file will be located to initiate failover. | yes | /tmp/postgresql.trigger.5432 |
| **pg_replication_backup** | The choice to run a backup before configuration is done | yes | true |
| **pg_replication_backup_loc** | Where the backup files will be stored on the remote postgresql server | yes | /tmp |
| **pg_replication_user** | The user that will be used for replication | yes | repuser |

## Dependencies

None.

## Example Playbook

    - hosts: database
      become: yes
      vars_files:
        - vars/main.yml
      roles:
        - ansible-role-pg-failover

*Inside `vars/main.yml`*:
    pg_replication_user: towerrepuser
    pg_replication_db: tower
    pg_replication_master: rhel8-postgresql01  
    pg_replication_backup: true


## License

MIT / BSD

## Author Information

This role was created in the darkest period of 2020 by [Ken Hitchcock](https://github.com/kenhitchcock).
