FROM ghcr.io/openclaw/openclaw:2026.4.2@sha256:0b2170d5ec3a487a6313ed0556d377c5c5c80a0f806043daa2e685a4bedd45e3
USER root
RUN apt-get update && apt-get install -y --no-install-recommends \
    ffmpeg \
    && rm -rf /var/lib/apt/lists/*
USER node
