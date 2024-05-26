# setup-postgis

[![GitHub](https://img.shields.io/badge/github-nyurik/action--setup--postgis-8da0cb?logo=github)](https://github.com/nyurik/action-setup-postgis)
[![CI build](https://github.com/nyurik/action-setup-postgis/actions/workflows/ci.yml/badge.svg)](https://github.com/nyurik/action-setup-postgis/actions)
[![Marketplace](https://img.shields.io/badge/market-setup--postgis-6F42C1?logo=github)](https://github.com/marketplace/actions/setup-postgresql-and-postgis-for-linux-macos-windows)

> [!TIP]
>
> PostgreSQL installation is done by the `ikalnytskyi/action-setup-postgres` action.  All parameters are passed as is to the original action, with the addition of the `cached-dir` parameter to specify where to download and cache PostGIS binaries. See the [original documentation](https://github.com/ikalnytskyi/action-setup-postgres) for more details.

* Runs on all Linux, macOS and Windows GitHub runners
* Installs the correct version of PostGIS and runs `CREATE EXTENSION postgis` in the new database.
  * Linux version is installed from the [PostGIS apt repository](https://postgis.net/install/).
  * Windows version is installed from the [OSGeo](https://download.osgeo.org/postgis/windows/).
  * MacOS version is installed using [Homebrew package](https://formulae.brew.sh/formula/postgis).

See also [action-setup-nginx](https://github.com/nyurik/action-setup-nginx) to configure NGINX service.

## Usage

```yaml
steps:
  - uses: nyurik/action-setup-postgis@v2
    id: postgres

  - name: Test PostGIS is installed
    run: psql -v ON_ERROR_STOP=1 -c 'SELECT PostGIS_Full_Version();' "$CONNECTION_STR"
    env:
      CONNECTION_STR: ${{ steps.postgres.outputs.connection-uri }}
```

#### Input parameters

| Key        | Value                                                                                                | Default     |
|------------|------------------------------------------------------------------------------------------------------|-------------|
| username   | The username of the user to setup.                                                                   | `postgres`  |
| password   | The password of the user to setup.                                                                   | `postgres`  |
| database   | The database name to setup and grant permissions to created user.                                    | `postgres`  |
| port       | The server port to listen on.                                                                        | `5432`      |
| cached-dir | Where should the temporary downloads be placed. Used to download and cache PostGIS binary.           | `downloads` |

#### Outputs

| Key            | Description                                  | Example                                             |
|----------------|----------------------------------------------|-----------------------------------------------------|
| connection-uri | The connection URI to connect to PostgreSQL. | `postgresql://postgres:postgres@localhost/postgres` |
| service-name   | The service name with connection parameters. | `postgres`                                          |

#### User permissions

| Key         | Value |
|-------------|-------|
| usesuper    | true  |
| usecreatedb | true  |

## License

The scripts and documentation in this project are released under the
[MIT License](LICENSE).
