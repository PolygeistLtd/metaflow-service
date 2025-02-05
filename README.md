# Metaflow Local Environment

This is an implementation of Metaflow that runs entirely on a local machine, with no access to cloud services. It is meant to for local development as well production workloads that are not hosted on a networked machine.

## Requirements

- Docker and docker compose
- An internet connection to pull down containers and UI code
- git for building the UI code
- make sure localhost ports 80,8080, 8083, and 9090 are not used by something else

## Installation

### 1. Build the UI container

Most containers used in this installation are publicly available via Docker Hub, however the UI container is not and must be built from source.

To build the container:

1. In a directory that is not the root of the directory of the project clone the repo using the command:
``` bash
git clone https://github.com/Netflix/metaflow-ui.git
```
2. Enter the repo directory and build the container.
```bash
cd metaflow-ui
docker build --tag metaflow-ui:latest .
```

### Start the Docker Compose environment.

Navigate to the root directory of the project and run the command:
```
docker compose up -d
```

This will start the environment. Once the Docker Compose Environment is running, you can navigate to the Metaflow UI in a browser at [http://localhost](http://localhost)

Check to see that the UI has successfully connected to the back end by confirming the light at the top right of the screen is green.

## Using Local Metaflow

To use the local Metaflow setup for flows put the following configuration in `~/.metaflowconfig/config.json`. This should replace any previous Metaflow configuration.

```json
{
    "METAFLOW_DATASTORE_SYSROOT_S3": "s3://metaflow_data/metaflow",
    "METAFLOW_DATATOOLS_S3ROOT": "s3://metaflow_data/data",
    "METAFLOW_DEFAULT_DATASTORE": "s3",
    "METAFLOW_S3_ENDPOINT_URL": "http://localhost:9090",
    "METAFLOW_DEFAULT_METADATA": "service",
    "METAFLOW_SERVICE_INTERNAL_URL":  "http://localhost:80/meta",
    "METAFLOW_SERVICE_URL": "http://localhost:80/meta",
    "METAFLOW_AWS_ACCESS_KEY_ID":"test",
    "METAFLOW_AWS_SECRET_ACCESS_KEY":"test",
    "METAFLOW_AWS_DEFAULT_REGION":"us-east-1"
}
```

## Thanks to watch out for

- This system proxies everything through nginx. There are commented out port assignments if you want to interface with containers directly
- The flow metadata is stored in a Postgres instance backed by a volume. this volume is deleted once the system is taken down. If you would like to persist flow metadata, use a more permanent volume or folder.
