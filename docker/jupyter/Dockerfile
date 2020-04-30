# Choose your desired base image
ARG BASE_CONTAINER=jupyter/minimal-notebook:latest
FROM $BASE_CONTAINER

USER root

# Remove the manpage blacklist, install man, install docs
RUN rm /etc/dpkg/dpkg.cfg.d/excludes \
    && apt-get update \
    && apt-get -yq upgrade \
    && apt-get install -yq libpq-dev iputils-ping \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

USER $NB_UID