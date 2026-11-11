# Changelog

All notable changes to the `mariuszczyz.mariadb` collection are documented
here. This project adheres to [Semantic Versioning](https://semver.org/).

## [2.0.0]

### Breaking changes

- Converted the standalone `mariuszczyz.centos-mysql` role into the
  `mariuszczyz.mariadb` collection. The role is now
  `mariuszczyz.mariadb.mariadb`.
- The root password is now the required role variable
  `mariadb_root_password` (supply via ansible-vault). The legacy
  `MYSQL_ROOT_PASSWORD` variable and the committed plaintext
  `vars/secret-vault.yml` have been removed.
- Dropped EL7/CentOS 7 support. EL9 and EL10 are now required.

### Added

- Jinja2 configuration template (`server.cnf.j2`) with parameterized
  low-memory tuning.
- `meta/argument_specs.yml` documenting every role variable.
- Molecule test scenario (podman, Rocky 9) and GitHub Actions CI.

### Changed

- Package manager `yum` -> `dnf`; connector `MySQL-python` -> `python3-PyMySQL`.
- SQL hardening now uses the `ansible.mariadb` modules
  (`mariadb_user`, `mariadb_db`, `mariadb_query`) against the
  `mysql.global_priv` table.
- Removed the duplicate `key_buffer_size`, the conflicting
  `table_cache`/`table_open_cache` pair, and the deprecated
  `query_cache_*` options from the server configuration.
- Raised `thread_stack` to 256K. The legacy 128K is below the minimum
  MariaDB 10.5 needs to initialize the data directory, so the server
  aborted on first start.
