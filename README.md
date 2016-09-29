# Pacifica Core Services
=============
Pacifica Data Management System for Scientific Data Archive.

## Core Services

The core services are split up into several git submodules. These can
be forked or replaced with custom version as a particular site might
want.

 - Uploader - Data uploader for collecting data from generators and
   adding metadata.
 - Upload Status - Status tools for when uploads have succeeded from
   a specific uploader.
 - Ingest - Validates incoming data and stores the data to the
   archive.
 - Reporting - Provides overall views, statistics and spread sheets
   of data coming in from generators.
 - Archive Interface - This is the Pacifica interface to the archive
   supports data on disk or tape.
 - Policy - This defines the policy code that sites might customize
   to change behavior of the other services.
 - UniqueID - This defines the unique identifiers used in the rest
   of the services (ingest and cart)
 - Searching - Dynamic search capabilities with an emphasis on data
   discovery.
 - Cart - Data cart for requesting data to be bundled and be made
   available for download later.

## Docker Compose Environment

Docker compose is used heavily to deploy developer environments for
interacting with the services one at a time. The primary
`docker-compose.yml` in this repository pulls images from
[docker hub](https://hub.docker.com/u/pacifica/dashboard) and creates
all the services and dependencies.

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
