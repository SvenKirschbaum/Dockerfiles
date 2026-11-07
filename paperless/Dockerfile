FROM ghcr.io/paperless-ngx/paperless-ngx:3.4.2@sha256:d704e84f88300eb6ab001ab15720a2330ff99169cf9f94d6b8fcc44b63a84c84

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
