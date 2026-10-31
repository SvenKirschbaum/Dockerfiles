FROM ghcr.io/paperless-ngx/paperless-ngx:3.4.1@sha256:1a08d8dfc2b51a08de09652dd86ad4d0c611c362382f944533ad38b73125fbc4

USER root

ARG DEBIAN_FRONTEND=noninteractive

RUN set -eux; \
    apt-get update; \
    apt-get upgrade --yes --quiet; \
    apt-get clean --yes; \
    rm -rf /var/lib/apt/lists/* /var/cache/apt/archives/*

COPY constraints.txt /usr/local/share/paperless-overrides/constraints.txt

RUN set -eux; \
    python3 -m pip install --upgrade --requirement /usr/local/share/paperless-overrides/constraints.txt; \
    python3 -m pip check; \
    rm -rf /root/.cache/pip
