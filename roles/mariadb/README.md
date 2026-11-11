# mariuszczyz.mariadb.mariadb

Installs and configures a MariaDB server on EL9+ (Rocky Linux, AlmaLinux):
installs the server packages, renders a low-memory configuration, starts the
service, sets the root password, and runs the standard hardening steps
(remove anonymous users, disallow remote root, drop the `test` database).

The root password is supplied via the required `mariadb_root_password`
variable (use ansible-vault). See `meta/argument_specs.yml` for the full,
authoritative list of role variables, their types, and defaults.
