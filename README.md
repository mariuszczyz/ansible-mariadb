# mariuszczyz.mariadb

This collection installs and configures a MariaDB server on Enterprise Linux 9
and 10 (Rocky Linux, AlmaLinux) and Fedora 42+. The shipped configuration uses
low-memory settings for servers with less than 2 GB of available memory.

This collection replaces the legacy `mariuszczyz.centos-mysql` role. It targets
EL9+ and uses dnf. It uses the `PyMySQL` connector and the `ansible.mariadb`
modules. The root password is a required role variable. The legacy role kept it
in a committed vars file.

## Supported platforms

| Platform | Supported versions | Distribution MariaDB stream | Molecule tested |
| --- | --- | --- | --- |
| Rocky Linux | 9, 10 | 10.5 (EL9), 10.11 (EL10) | 9 |
| AlmaLinux | 9, 10 | 10.5 (EL9), 10.11 (EL10) | — |
| Fedora | 42+ | 10.11 (42, 43), 11.8 (44), 12.3 (45) | 42, 44 |

## Requirements

- ansible-core >= 2.16
- Collection dependencies:

```bash
ansible-galaxy collection install -r requirements.yml
```

## Usage

```yaml
- hosts: servers
  roles:
    - role: mariuszczyz.mariadb.mariadb
```

## Variables

The role requires `mariadb_root_password`. That variable has no default. Every
other variable is optional and defaults as shown.

| Variable | Default | Description |
| --- | --- | --- |
| `mariadb_root_password` | _(required)_ | MariaDB root password. Supply it with ansible-vault. |
| `mariadb_packages` | `[mariadb-server, mariadb, python3-PyMySQL]` | Packages to install. |
| `mariadb_service_name` | `mariadb` | MariaDB service unit name. |
| `mariadb_socket` | `/var/lib/mysql/mysql.sock` | Local socket path. |
| `mariadb_config_file` | `/etc/my.cnf.d/server.cnf` | Where the role writes the rendered server config. |
| `mariadb_root_clients` | `[localhost, 127.0.0.1, ::1]` | Hosts provisioned for the root account. |
| `mariadb_port` | `3306` | TCP listen port. |
| `mariadb_innodb_file_per_table` | `true` | Per-table InnoDB tablespaces. |
| `mariadb_performance_schema` | `false` | Performance schema setting. |
| `mariadb_key_buffer_size` | `4M` | MyISAM key buffer size. |
| `mariadb_max_allowed_packet` | `1M` | Max packet size. |
| `mariadb_table_open_cache` | `16` | Open tables cache. |
| `mariadb_sort_buffer_size` | `64K` | Sort buffer size. |
| `mariadb_read_buffer_size` | `256K` | Full-table-scan read buffer. |
| `mariadb_read_rnd_buffer_size` | `256K` | Sorted-read buffer. |
| `mariadb_net_buffer_length` | `2K` | Network buffer length. |
| `mariadb_thread_stack` | `256K` | Per-thread stack size. |
| `mariadb_server_id` | `1` | Replication server id. |
| `mariadb_wait_timeout` | `120` | Noninteractive connection timeout (s). |
| `mariadb_thread_cache_size` | `64` | Cached threads for reuse. |
| `mariadb_bulk_insert_buffer_size` | `4M` | MyISAM bulk-insert buffer. |
| `mariadb_tmp_table_size` | `4M` | The maximum size of an in-memory temporary table. |
| `mariadb_max_heap_table_size` | `4M` | The maximum size of a MEMORY table. |
| `mariadb_myisam_sort_buffer_size` | `2M` | MyISAM index-rebuild buffer. |
| `mariadb_innodb_buffer_pool_size` | `128M` | InnoDB buffer pool size. |
| `mariadb_innodb_log_file_size` | `16M` | InnoDB redo log file size. |
| `mariadb_innodb_log_buffer_size` | `24M` | InnoDB log buffer size. |
| `mariadb_innodb_flush_log_at_trx_commit` | `1` | How InnoDB flushes the log at commit. |
| `mariadb_innodb_lock_wait_timeout` | `50` | InnoDB row-lock wait timeout (s). |

### Supplying the root password

The collection does not store the root password. Define it in a group_vars file.
Encrypt that file with ansible-vault:

```bash
# group_vars/servers/vault.yml
#   mariadb_root_password: "your-secret-password"
ansible-vault encrypt group_vars/servers/vault.yml
```

Run the playbook with `--ask-vault-pass` (or a vault password file).

> **Breaking change:** The role now requires `mariadb_root_password`. The old
> name was `MYSQL_ROOT_PASSWORD`. Version 2.0.0 removed the committed plaintext
> `vars/secret-vault.yml` that held it.

## License

MIT

## Author

Mariusz Czyz
