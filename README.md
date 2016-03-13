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
 - Searching - Dynamic search capabilities with an emphasis on data
   discovery.
 - Cart - Data cart for requesting data to be bundled and be made
   available for download later.

