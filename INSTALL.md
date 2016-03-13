# Pacifica Core Service Installation

The projects that make up the core have their own individual install
documents that need to be followed. This document describes how one
might fit all the projects together into a working system. We'll
focus on several platforms where Pacifica will be supported.

## Docker Installation

The focus for installing Pacifica in a Docker environment is two
fold. First is single system integration testing or development of
the various projects. Second is for deployment of the system in cloud
like environments that run Docker containers. These environments have
limitations inherent in their architecture and those should be taken
into consideration before running Pacifica in those environments.

## Cloud Installations

The focus for Cloud installation is for production usage. The primary
users of this system will have data ranging from terabytes to 
petabytes. So, private and public clouds should be expected.
Furthermore, integration with proprietary data archive solutions
should also be expected.
