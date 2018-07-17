# Pacifica Core Services
[![Build Status](https://travis-ci.org/pacifica/pacifica.svg?branch=master)](https://travis-ci.org/pacifica/pacifica)
Pacifica is an open source scientific data management platform for 
harvesting, validating, and distributing data and metadata. It is
architected as a flexible set of inter-changeable tools used to build
custom scientific data management solutions to meet the diverse
changing demands of research at different institutions.

## Core Services

The core services are split up into several git submodules. These can
be forked or replaced with custom version as a particular site might
want.

 - Ingest - Validates incoming data and stores the data to the
   archive.
 - Archive Interface - This is the Pacifica interface to the archive
   supports data on disk or tape.
 - Policy - This defines the policy code that sites might customize
   to change behavior of the other services.
 - UniqueID - This defines the unique identifiers used in the rest
   of the services (ingest and cart)
 - Metadata - This defines the core metadata schema and access to
   meta in that schema.
 - Cart - Data cart for requesting data to be bundled and be made
   available for download later.

## Docker Compose Environment

Docker compose is used heavily to deploy developer environments for
interacting with the services one at a time. The primary
`docker-compose.yml` in this repository pulls images from
[docker hub](https://hub.docker.com/r/pacifica/) and creates
all the services and dependencies.

For more information about docker compose environment refer to the
[documentation](DOCKER_COMPOSE.md).

## Code Standards and Architectures

Coding standards are enforced by Travis-CI. They will be checked and
commented on by merge requests.

[Code Climate](https://www.codeclimate.com) is used to inform on code
coverage and potential issues through static code analysis.

Architectures are talked about in detail in the ```docs``` submodule.
However, there are two basic types of tools, Python REST services and
PHP/JavaScript Websites.

## Contributing

Contributing is pretty basic, fork it on github and create a merge
request. Further reading is done [here](https://help.github.com/articles/using-pull-requests/)
