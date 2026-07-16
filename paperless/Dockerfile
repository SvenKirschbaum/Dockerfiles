FROM ghcr.io/paperless-ngx/paperless-ngx:2.20.15@sha256:6c86cad803970ea782683a8e80e7403444c5bf3cf70de63b4d3c8e87500db92f

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
