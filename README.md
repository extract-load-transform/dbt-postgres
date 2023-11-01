# Look Ma, No cloud dbt

Setup environment for `dbt` with local `postgres` database instead cloud (e.g. `BigQuery`).

The environment setup
- docker-compose environment with following services
  - `dbt`: docker with `dbt` that can run on `Mac` `ARM` achitecture (`xemuliam/dbt`)
  - `db`: postgres database

`dbt` docker needs to install:
- `postgres-client` to talk to `postgres` at `db`
- `dbt-postgress` adaptor

Verify the environment when start-up
```bash
dbt --version
```

## directory structure

```text
.
├── .dbt
│   └── profiles.yml
├── .devcontainer
│   ├── Dockerfile
│   ├── devcontainer.json
│   └── docker-compose.yml
├── README.md
└── projects
    └── my_project
        ├── ...
        └── ...
```

## Create dbt project
```bash

dbt init my_project
# ...
# Which database would you like to use?
# [1] bigquery
# [2] postgres

# (Don't see the one you want? https://docs.getdbt.com/docs/available-adapters)
Enter a number: 2
# Profile my_project written to /home/user/.dbt/profiles.yml using target's sample configuration. Once updated, you'll be able to start developing with dbt.

# move dbt profile to repo so that it can saved in repository
cp -r /home/user/.dbt .dbt

mkdir projects
mv my_project projects

```

```bash
dbt debug --profiles-dir ../../.dbt --target dev

dbt run --profiles-dir ../../.dbt --target dev

dbt test --profiles-dir ../../.dbt --target dev
```

```bash
PGPASSWORD=postgres psql -h db -U postgres

postgres=# \c
psql (15.3 (Debian 15.3-0+deb12u1), server 16.0 (Debian 16.0-1.pgdg120+1))
WARNING: psql major version 15, server major version 16.
         Some psql features might not work.
You are now connected to database "postgres" as user "postgres".

postgres=# select current_database();
 current_database 
------------------
 postgres
(1 row)

postgres=# \dt
                List of relations
  Schema  |        Name        | Type  |  Owner   
----------+--------------------+-------+----------
 postgres | my_first_dbt_model | table | postgres
(1 row)

postgres=# select * from my_first_dbt_model;
 id 
----
  1
(1 row)

postgres-# \q
```

## Reference:
- https://towardsdatascience.com/getting-hands-on-with-dbt-data-build-tool-a157d4151bbc
- https://github.com/microsoft/vscode-dev-containers/tree/main/containers/python-3-postgres/.devcontainer
