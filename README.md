# PGBouncer-Reloader

## Why ?

PgBouncer will be to be the main access for various postgresql containers, the instances don't need to expose any ports.
Reloading and maintaining manually the configuration is time consuming, with this tool it will be updated each time a new container is running.
This is not a tool for production but local developments.

## How ?

### Labels

Use labels in your docker configuration for each postgresql instances.

```
labels:
  - "pgbouncer.host=myserver"
  - "pgbouncer.dbname=mydatabase"
  - "pgbouncer.port=5432"
  - "pgbouncer.auth_user=myuser"
```

* `host` is optional, it will use the container_name by default
* `port` is optional, it will use `5432` by default
* `auth_user` is optional, the user `postgres` is always allowed
* `dbname` can be csv list, you'll need to define all the dbs (postgres excluded)

### Network

PgBouncer use a network named `db_network`, you'll need to create it and add it as external

On top

```
networks:
    db_network:
        external: true
    ...
```

In the container configuration

```
networks:
    - db_network
    - ...
```

### Ports

If you're exposing a port, you should use a different one than pgbouncer.
Either keep it empty and let docker choose for you, or define one using an env var, like `$PGHOST_PORT`.

```
ports:
    - "${PGHOST_PORT-}:5432"
```
Doing this, you could use the expected port `5432`, when not using this tool.

## Important

If you're running a local instance of postgresql it will get conflicted because of the port.
You can change it by editing the value in `docker-compose.yml` and `pgbouncer.tmpl` files.
