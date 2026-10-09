FROM ghcr.io/paperless-ngx/paperless-ngx:3.3.0@sha256:6b94799bc769a063b3340cc5847402e626d4d91d29a295a8e747f8e9bead4207

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
