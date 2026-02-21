FROM ghcr.io/openclaw/openclaw:2026.2.21@sha256:ce271192cd70250d16fc5911903d9953467a40faf8b34e87cbd042e6b49b6036
USER root
RUN apt-get update && apt-get install -y --no-install-recommends \
    ffmpeg \
    && rm -rf /var/lib/apt/lists/*
USER node
