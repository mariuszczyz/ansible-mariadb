# Changelog

This file lists the notable changes to the `mariuszczyz.mariadb` collection.
The project follows [Semantic Versioning](https://semver.org/).

## [2.1.0]

### Added

- Fedora support for Fedora 42+. Tested on Fedora 42 and 44. Fedora ships
  MariaDB as the unversioned `mariadb-server` and `mariadb` packages. Fedora
  uses the `mariadb` service unit, the `/etc/my.cnf.d/` layout, and the
  `/var/lib/mysql/mysql.sock` socket. The role needed no code changes.
- Molecule instances for Fedora 42 and 44 (podman).

## [2.0.0]

### Breaking changes

- Converted the standalone `mariuszczyz.centos-mysql` role into the
  `mariuszczyz.mariadb` collection. The role is now
  `mariuszczyz.mariadb.mariadb`.
- The role now requires the root password as the variable
  `mariadb_root_password`. Supply it with ansible-vault. Version 2.0.0 removed
  the legacy `MYSQL_ROOT_PASSWORD` variable and the committed plaintext
  `vars/secret-vault.yml`.
- Dropped EL7/CentOS 7 support. The role now requires EL9 and EL10.

### Added

- Jinja2 template `server.cnf.j2`. It renders the configuration from the role
  variables, with low-memory defaults.
- `meta/argument_specs.yml` documenting every role variable.
- Molecule test scenario (podman, Rocky 9) and GitHub Actions CI.

### Changed

- Changed the package manager from `yum` to `dnf`. Changed the connector from
  `MySQL-python` to `python3-PyMySQL`.
- SQL hardening now uses the `ansible.mariadb` modules
  (`mariadb_user`, `mariadb_db`, `mariadb_query`) against the
  `mysql.global_priv` table.
- Removed from the server configuration:
  - the duplicate `key_buffer_size` entry
  - the conflicting `table_cache`/`table_open_cache` pair. The template now sets
    `table_open_cache` once, from `mariadb_table_open_cache`.
  - the deprecated `query_cache_*` options
- Raised `thread_stack` to 256K. The old value, 128K, is below the minimum
  MariaDB 10.5 needs to initialize the data directory. The server aborted on
  the first start.
