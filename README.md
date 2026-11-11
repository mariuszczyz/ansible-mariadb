# mariuszczyz.mariadb — MariaDB server for EL9+ and Fedora

MariaDB server installation and configuration for Enterprise Linux 9 and 10
(Rocky Linux, AlmaLinux) and Fedora 42+. The included configuration is tuned
for low-resource servers with less than 2 GB of available memory.

This collection replaces the legacy `mariuszczyz.centos-mysql` role. It targets
EL9+ (dnf, MariaDB 10.5/11.x), uses the `PyMySQL` connector and the
`ansible.mariadb` modules, and takes the root password as a required role
variable instead of a committed vars file.

## Requirements

- ansible-core >= 2.16
- Target hosts running EL9/EL10 (Rocky Linux, AlmaLinux) or Fedora 42+
  (tested: Fedora 42, 44)
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

`mariadb_root_password` is **required** and has no default. All other variables
have the defaults shown and are optional.

| Variable | Default | Description |
| --- | --- | --- |
| `mariadb_root_password` | _(required)_ | MariaDB root password. Supply via ansible-vault. |
| `mariadb_packages` | `[mariadb-server, mariadb, python3-PyMySQL]` | Packages to install. |
| `mariadb_service_name` | `mariadb` | MariaDB service unit name. |
| `mariadb_socket` | `/var/lib/mysql/mysql.sock` | Local socket path. |
| `mariadb_config_file` | `/etc/my.cnf.d/server.cnf` | Rendered server config destination. |
| `mariadb_root_clients` | `[localhost, 127.0.0.1, ::1]` | Hosts provisioned for the root account. |
| `mariadb_port` | `3306` | TCP listen port. |
| `mariadb_innodb_file_per_table` | `true` | Per-table InnoDB tablespaces. |
| `mariadb_performance_schema` | `false` | Enable performance schema. |
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
| `mariadb_tmp_table_size` | `4M` | In-memory temp table max size. |
| `mariadb_max_heap_table_size` | `4M` | MEMORY table max size. |
| `mariadb_myisam_sort_buffer_size` | `2M` | MyISAM index-rebuild buffer. |
| `mariadb_innodb_buffer_pool_size` | `128M` | InnoDB buffer pool size. |
| `mariadb_innodb_log_file_size` | `16M` | InnoDB redo log file size. |
| `mariadb_innodb_log_buffer_size` | `24M` | InnoDB log buffer size. |
| `mariadb_innodb_flush_log_at_trx_commit` | `1` | InnoDB commit flush policy. |
| `mariadb_innodb_lock_wait_timeout` | `50` | InnoDB row-lock wait timeout (s). |

### Supplying the root password

The root password is not stored in the collection. Define it in a vaulted
group_vars file and encrypt it:

```bash
# group_vars/servers/vault.yml
#   mariadb_root_password: "your-secret-password"
ansible-vault encrypt group_vars/servers/vault.yml
```

Run the playbook with `--ask-vault-pass` (or a vault password file).

> **Breaking change:** the legacy variable `MYSQL_ROOT_PASSWORD` (previously
> committed in `vars/secret-vault.yml`) is renamed to `mariadb_root_password`
> and is now required. The committed plaintext file has been removed.

## License

MIT

## Author Information

Author: Mariusz Czyz
