FROM ghcr.io/paperless-ngx/paperless-ngx:3.5.1@sha256:8508853b8a0f8394074260957d0be186c8c64f346973b24a7583de4580f306b9

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
