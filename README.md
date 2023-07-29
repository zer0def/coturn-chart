# coturn helm chart
An unofficial [coturn](https://github.com/coturn/coturn) helm chart using the official [coturn docker image](https://hub.docker.com/r/coturn/coturn).

# Usage

## TLDR 
Note that you still need to fill out the [`charts/coturn/values.yaml`](./charts/coturn/values.yaml) (Autogenerated Docs can be found in [`charts/coturn/README.md`](./charts/coturn/README.md)).

```console
helm repo add coturn https://jessebot.github.io/coturn-chart/
helm install coturn coturn --values values.yaml
```

## Basics

### Coturn Realm
At very least, you'll need to configure a coturn [realm](https://github.com/coturn/coturn/blob/d7db17f048675f46fc2b30827813eeaf0c822fb2/examples/etc/turnserver.conf#L349-L358) which is like your hostname, and is used for authentication as well.

```yaml
# most coturn config parameters that you really need
coturn:
  # --  hostname for the coturn server realm
  realm: "turn.example.com"
```

### Adding a user declaritively
Pass in one set of credentials (username/password) directly either plaintext, or via an existing k8s secret, like this:

```yaml
# most coturn config parameters that you really need
coturn:
  # --  hostname for the coturn server realm
  realm: "turn.example.com"

  auth:
    # -- username for the main user of the turn server; ignored if you existingSecret is not ""
    username: "coturn"
    # -- password for the main user of the turn server; ignored if you existingSecret is not ""
    password: "myverysecretpasswordthatimobviouslygoingtochangeright"
    # -- existing secret with keys username/password for coturn; if this is not "" then we will ignore coturn.auth.username/password
    existingSecret: ""
    secretKeys:
      # -- key in existing secret for turn server user
      username: username
      # -- key in existing secret for turn server user's password
      password: password
```

Currently only one user is supported, but we'd like to support adding more than that to match what is possible in the [coturn/coturn:`examples/etc/turnserver.conf`](https://github.com/coturn/coturn/blob/d7db17f048675f46fc2b30827813eeaf0c822fb2/examples/etc/turnserver.conf#L256-L280)

### Databases

### Internal SQLite database
If you would like to use the built-in sqlite database, set `externalDatabse.enabled` and `postgresql.enabled` to `false` in your `values.yaml` like this:
```yaml
externalDatabse:
  enabled: false
postgresql:
  enabled: false
```

### Bundled PostgreSQL subchart
We provide optional Bitnami Postgresql subchart to deploy an external database. You can use it like this:

```yaml
externalDatabse:
  enabled: true
postgresql:
  enabled: false
  global:
    postgresql:
      # -- global.postgresql.auth overrides postgresql.auth
      auth:
        # -- username for database, ignored if existingSecret is passed in
        username: "coturn"
        # -- password for db, autogenerated if empty & existingSecret empty
        password: "mycoolpasswordthatisplaintextforsomereason"
        # -- database to create, ignored if existingSecret is passed in
        database: "coturn"
        # -- name of existing Secret to use for postgresql credentials
        existingSecret: ""
        # Names of the keys in existing secret to use for PostgreSQL credentials
        # all of these are ignored if existingSecret is empty
        secretKeys:
          # -- key in existingSecret for database to create
          hostname: "hostname"
          # -- key in existingSecret for database to create
          database: "database"
          # -- key in exsiting Secret to use for the coturn user
          username: "username"
          # -- key in existing Secret to use for postgres admin user's password
          adminPasswordKey: "postgresPassword"
          # -- key in existing Secret to use for coturn user's password
          userPasswordKey: "password"
```

You're free to use any other values you find in the [Bitnami postgresql helm values](https://github.com/bitnami/charts/tree/main/bitnami/postgresql) under the `postgresql` parameter in your values.yaml for coturn.

### External PostgreSQL database
If `externalDatabase.enabled` is set to `true`, and `postgresql.enabled` is set to false, you can pass in credentials from an existing postgresql database, like this:

```yaml
externalDatabse:
  enabled: true
  # -- Currently only postgresql is supported. mariadb/mysql coming soon
  type: "postgresql"
  # -- required if externalDatabase.enabled: true and postgresql.enabled: false
  hostname: "mypostgresserver"
  # -- username for database, ignored if existingSecret is passed in
  username: "coturn"
  # -- password for database, ignored if existingSecret is passed in
  password: "coolpasswordfordogs"
  # -- database to create, ignored if existingSecret is passed in
  database: "coturn"
  # -- name of existing Secret to use for postgresql credentials
  existingSecret: ""
  # Names of the keys in existing secret to use for PostgreSQL credentials
  secretKeys:
    # -- key in existing Secret to use for the db user
    username: ""
    # -- key in existing Secret to use for db user's password
    password: ""
    # -- key in existing Secret to use for the database name
    database: ""
    # -- key in existing Secret to use for the db's hostname
    hostname: ""
postgresql:
  enabled: false
```

## Status and Contributing
This is actively maintained by both live developer and [renovate](https://github.com/renovatebot/github-action) via a scheduled Github Action. If you'd like to contribute, please read the [CONTRIBUTING.md](./CONTRIBUTING.md) feel free to open a PR :) If you'd like a feature or want to report a bug, please do that in the GitHub Issues. If you know coturn and k8s well enough, please also feel free to scan the issues and help others <3

## Thanks
This is a fork of the now deprecated [iits-consulting/coturn](https://github.com/iits-consulting/coturn-chart) chart. Thanks to them for getting this started.
