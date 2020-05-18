# Docker Compose Development Environment

Docker compose is a tool to spawn multiple docker 
containers and link them together on a single docker server
or docker swarm cluster. This is useful for developers
to bring up the environment or for demonstration purposes
to inspect the system.

## Docker Compose Details

The docker compose file defines five volumes. The volumes
are usually located on the host in
`/var/lib/docker/volumes`. These volumes define the
persistent state across reboots of the system. The location
maybe distribution dependent so check the docker provider
documentation to be certain.

The `archivedata` volume contains the files in the archive.
These files will be saved for the life of the volume. This
does happen across bring up and shutdown of the services.

The `ingestshared` volume is the intermediate storage used
by the ingest services. Data from the uploaders are staged
on the `ingestshared` volume.

The `cartshared` volume is the intermediate storage used by
the cart service. Data from the archive is staged on the
`cartshared` volume.

The `metadata` and `uniqueiddata` volumes are the 
persistent metadata databases required by the services.

Each service in docker compose has its external port
exposed so you can interact with it locally.

| Port | Service            |
| ---- | ------------------ |
| 8080 | Archive Interface  |
| 8081 | Cart Interface     |
| 8051 | UniqueID Interface |
| 8121 | Metadata Interface |
| 8181 | Policy Interface   |
| 8066 | Ingest Interface   |
| 8180 | Proxy Interface    |
| 8888 | Jupyter Notebook   |

## Host Requirements

Requirements for the host running the docker server are the
following.

 * RAM - 8Gb minimum - 16Gb preferred
 * CPUs - 4 minimum - 8 preferred
 * DISK - 20Gb minimum - 1Tb preferred

The disk requirement is very dependent on the data sets
that will be uploaded. Scale that as nessisary for testing
and development purposes.

## Bring Up Services

There are two commands to separate the bring up of Pacifica
with docker compose.

```
# docker-compose pull
# docker-compose up
```

Running `docker-compose up` does perform a pull prior to
bringing up the services. However, depending on I/O and
network bandwidth, services may fail to connect.

Verify the services are running.

```
# docker-compose ps
```

## Test the Interfaces with Jupyter

We include a Jupyter container with some pre-loaded notebooks to
verify the services are running. Follow the web link
[here](http://localhost:8888) to find the examples and 
documentation to verify the services are running.

### The Examples Notebook

This notebook shows off the basics of the API for each service
and how to interface with them from a client code. The system
loads test data from the notebook to show end-to-end functionality.

### The Notifications Notebook

This notebook shows off the Notifications API and how to subscribe to
and receive data from the system. The example shows a worker that
converts all text files to uppercase within a dataset and uploads the
converted data.

## Inspect the Metadata with React

We also include a React JS based management tool to inspect the
metadata in the Metadata service. The service is available on
[localhost:8889](http://localhost:8889) and runs through an NGINX
proxy. You can watch and manipulate the metadata as you are running
through the example notebooks.
