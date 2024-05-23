# action-setup-postgis

[![GitHub](https://img.shields.io/badge/github-nyurik/action--setup--postgis-8da0cb?logo=github)](https://github.com/nyurik/action-setup-postgis)
[![CI build](https://github.com/nyurik/action-setup-postgis/actions/workflows/ci.yml/badge.svg)](https://github.com/nyurik/action-setup-postgis/actions)
[![Marketplace](https://img.shields.io/badge/market-action--setup--postgis-6F42C1?logo=github)](https://github.com/marketplace/actions/setup-postgis-service-for-linux-macos-windows)

This GitHub action sets up a PostgreSQL server with PostGIS extension. The code is based on
the [ikalnytskyi/action-setup-postgres](https://github.com/ikalnytskyi/action-setup-postgres) action.

* Runs on Linux, macOS and Windows GitHub runners.
* Adds PostgreSQL [binaries](https://www.postgresql.org/docs/current/reference-client.html) (e.g. `psql`) to `PATH`.
* Uses PostgreSQL installed in [GitHub Actions Virtual Environments](https://github.com/actions/virtual-environments).
* Installs the correct version of PostGIS and runs `CREATE EXTENSION postgis` in the new database.
    * Linux version is installed from the [PostGIS apt repository](https://postgis.net/install/).
    * Windows version is installed from the [OSGeo](https://download.osgeo.org/postgis/windows/).
    * MacOS version is installed using [Homebrew package](https://formulae.brew.sh/formula/postgis).
* [Easy to check](action.yml) that IT DOES NOT contain malicious code.

See also [action-setup-nginx](https://github.com/nyurik/action-setup-nginx) to configure NGINX service.

## Usage

```yaml
steps:
  - uses: nyurik/action-setup-postgis@v1

  - run: psql "$DB_CONN_STR" -c 'CREATE DATABASE test;'
    env:
      DB_CONN_STR: ${{ steps.pg.outputs.connection-uri }}
```

#### Advanced

```yaml
steps:
  - uses: nyurik/action-setup-postgis@v1
    with:
      username: ci
      password: sw0rdfish
      database: test
      port: 34837
    id: pg

  - run: psql "$DB_CONN_STR" -c 'CREATE DATABASE test;'
    env:
      DB_CONN_STR: ${{ steps.pg.outputs.connection-uri }}
```

#### Input parameters

| Key        | Description                                                                                | Default    |
|------------|--------------------------------------------------------------------------------------------|------------|
| username   | The username of the user to setup.                                                         | postgres   |
| password   | The password of the user to setup.                                                         | postgres   |
| database   | The database name to setup and grant permissions to created user.                          | postgres   |
| port       | The server port to listen on.                                                              | 5432       |
| rights     | Space-separated list of params passed to `createuser`.                                     | --createdb |
| cached-dir | Where should the temporary downloads be placed. Used to download and cache PostGIS binary. | downloads  |

#### Outputs

| Key                                                       | Description                                         | Example |
|-----------------------------------------------------------|-----------------------------------------------------|---------|
| URI                                                       | connection-uri:                                     |         |
| description: The connection URI to connect to PostgreSQL. | `postgresql://postgres:postgres@localhost/postgres` |         |

## License

The scripts and documentation in this project are released under the
[MIT License](LICENSE).
