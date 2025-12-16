# nfrastack/container-meilisearch

## About

This repository will build a container for [meilisearch](https://www.meilisearch.com). A lightning-fast search engine API.

## Maintainer

- [Nfrastack](https://www.nfrastack.com)


## Table of Contents

- [About](#about)
- [Maintainer](#maintainer)
- [Table of Contents](#table-of-contents)
- [Installation](#installation)
  - [Prebuilt Images](#prebuilt-images)
  - [Quick Start](#quick-start)
  - [Persistent Storage](#persistent-storage)
- [Environment Variables](#environment-variables)
  - [Base Images used](#base-images-used)
  - [Core Configuration](#core-configuration)
  - [Meilisearch Configuration](#meilisearch-configuration)
- [Users and Groups](#users-and-groups)
- [Networking](#networking)
- [Maintenance](#maintenance)
  - [Shell Access](#shell-access)
- [Support & Maintenance](#support--maintenance)
- [License](#license)

## Installation

### Prebuilt Images
Feature limited builds of the image are available on the [Github Container Registry](https://github.com/nfrastack/container-meilisearch/pkgs/container/container-meilisearch) and [Docker Hub](https://hub.docker.com/r/nfrastack/meilisearch).

To unlock advanced features, one must provide a code to be able to change specific environment variables from defaults. Support the development to gain access to a code.

To get access to the image use your container orchestrator to pull from the following locations:

```
ghcr.io/nfrastack/container-meilisearch:(image_tag)
docker.io/nfrastack/meilisearch:(image_tag)
```

Image tag syntax is:

`<image>:<optional tag>-<optional_distribution>_<optional_distribution_variant>`

Example:

`ghcr.io/nfrastack/container-meilisearch:latest` or

`ghcr.io/nfrastack/container-meilisearch:1.0` or

`ghcr.io/nfrastack/container-meilisearch:1.0-alpine` or

`ghcr.io/nfrastack/container-meilisearch:alpine`

* `latest` will be the most recent commit
* An optional `tag` may exist that matches the [CHANGELOG](CHANGELOG.md) - These are the safest
* If it is built for multiple distributions there may exist a value of `alpine` or `debian`
* If there are multiple distribution variations it may include a version - see the registry for availability

Have a look at the container registries and see what tags are available.

#### Multi-Architecture Support

Images are built for `amd64` by default, with optional support for `arm64` and other architectures.

### Quick Start

* The quickest way to get started is using [docker-compose](https://docs.docker.com/compose/). See the examples folder for a working [compose.yml](examples/compose.yml) that can be modified for your use.

* Map [persistent storage](#persistent-storage) for access to configuration and data files for backup.
* Set various [environment variables](#environment-variables) to understand the capabilities of this image.

### Persistent Storage

The following directories are used for configuration and can be mapped for persistent storage.

| Directory  | Description             |
| ---------- | ----------------------- |
| `/config/` | (optional) Config Files |
| `/data/`   | Data Files              |
| `/logs/`   | Logfiles                |

### Environment Variables

#### Base Images used

This image relies on a customized base image in order to work.
Be sure to view the following repositories to understand all the customizable options:

| Image                                                   | Description |
| ------------------------------------------------------- | ----------- |
| [OS Base](https://github.com/nfrastack/container-base/) | Base Image  |

Below is the complete list of available options that can be used to customize your installation.

* Variables showing an 'x' under the `Advanced` column can only be set if the containers advanced functionality is enabled.

#### Core Configuration

| Parameter                     | Description                                                            | Default                   | Advanced |
| ----------------------------- | ---------------------------------------------------------------------- | ------------------------- | -------- |
| `MEILISEARCH_SETUP_TYPE`      | Auto Configure Configuration each startup - Set to `MANUAL` to disable | `AUTO`                    |          |
| `CONFIG_PATH`                 | Path to configuration directory                                        | `/config/`                |          |
| `CONFIG_FILE`                 | Name of the config file                                                | `meilisearch.conf`        |          |
| `DATA_PATH`                   | Path to data directory                                                 | `/data/`                  |          |
| `DB_PATH`                     | Path to database file                                                  | `${DATA_PATH}/db/data.ms` |          |
| `DUMP_PATH`                   | Path to dump directory                                                 | `${DATA_PATH}/dumps/`     |          |
| `LOG_PATH`                    | Path to logs directory                                                 | `/logs/`                  |          |
| `LOG_LEVEL`                   | Log level                                                              | `info`                    |          |
| `SNAPSHOT_PATH`               | Path to snapshot directory                                             | `${DATA_PATH}/snapshots/` |          |
| `MEILISEARCH_ADDITIONAL_ARGS` | Additional arguments to pass to meilisearch execution                  |                           |          |

#### Meilisearch Configuration

| Parameter                      | Description                                               | Default               | Advanced | _FILE |
| ------------------------------ | --------------------------------------------------------- | --------------------- | -------- | ----- |
| `ENVIRONMENT`                  | Environment (`production`/`development`)                  | `production`          |          |       |
| `MASTER_KEY`                   | 16 byte Master Key used for securing API                  | ``                    |          | x     |
| `MEILISEARCH_USER`             | User running Meilisearch                                  | `meilisearch`         |          |       |
| `MEILISEARCH_GROUP`            | Group running Meilisearch                                 | `meilisearch`         |          |       |
| `LISTEN_IP`                    | IP address to listen on                                   | `0.0.0.0`             |          |       |
| `LISTEN_PORT`                  | Port to listen on                                         | `7700`                |          |       |
| `ENABLE_ANALYTICS`             | Enable/disable analytics                                  | `false`               |          |       |
| `ENABLE_METRICS`               | Enable/disable metrics                                    | `false`               | x        |       |
| `HTTP_PAYLOAD_SIZE_LIMIT`      | Max HTTP payload size                                     | `100MB`               |          |       |
| `IGNORE_DUMP_IF_DB_EXISTS`     | Ignore dump if DB exists                                  | `false`               |          |       |
| `IGNORE_MISSING_DUMP`          | Ignore missing dump                                       | `false`               |          |       |
| `IGNORE_MISSING_SNAPSHOT`      | Ignore missing snapshot                                   | `false`               |          |       |
| `IGNORE_SNAPSHOT_IF_DB_EXISTS` | Ignore snapshot if DB exists                              | `false`               |          |       |
| `MAX_INDEXING_MEMORY`          | Max memory for indexing (kB, 66% of available by default) | Calculated at runtime |          |       |
| `MAX_INDEXING_THREADS`         | Max threads for indexing                                  | `0`                   |          |       |
| `MAX_NUMBER_OF_BATCHED_TASKS`  | Max number of batched tasks (experimental)                | ``                    | x        |       |
| `REDUCE_INDEXING_MEMORY_USAGE` | Reduce memory usage during indexing (experimental)        | `false`               | x        |       |
| `SCHEDULE_SNAPSHOT`            | Enable scheduled snapshots                                | `false`               |          |       |
| `SSL_AUTH_PATH`                | Path for SSL client authentication                        | ``                    | x        |       |
| `SSL_CERT_PATH`                | Path to SSL certificate                                   | ``                    | x        |       |
| `SSL_KEY_PATH`                 | Path to SSL key                                           | ``                    | x        |       |
| `SSL_OCSP_PATH`                | Path to SSL OCSP file                                     | ``                    | x        |       |
| `SSL_REQUIRE_AUTH`             | Require SSL authentication                                | `false`               | x        |       |
| `SSL_RESUMPTION`               | Enable SSL session resumption                             | `false`               | x        |       |
| `SSL_TICKETS`                  | Enable SSL tickets                                        | `false`               | x        |       |

## Users and Groups

| Type  | Name          | ID   |
| ----- | ------------- | ---- |
| User  | `meilisearch` | 7700 |
| Group | `meilisearch` | 7700 |

### Networking

| Port   | Protocol | Description |
| ------ | -------- | ----------- |
| `7700` | tcp+udp  | Meilsearch  |

* * *

## Maintenance

### Shell Access

For debugging and maintenance, `bash` and `sh` are available in the container.

## Support & Maintenance

- For community help, tips, and community discussions, visit the [Discussions board](/discussions).
- For personalized support or a support agreement, see [Nfrastack Support](https://nfrastack.com/).
- To report bugs, submit a [Bug Report](issues/new). Usage questions will be closed as not-a-bug.
- Feature requests are welcome, but not guaranteed. For prioritized development, consider a support agreement.
- Updates are best-effort, with priority given to active production use and support agreements.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
