FROM ghcr.io/openclaw/openclaw:2026.3.12@sha256:c871ddf7ad1b4125218e5f010b59724f0d2bb299714c5bd6ba7c1e99462450a7
USER root
RUN apt-get update && apt-get install -y --no-install-recommends \
    ffmpeg \
    && rm -rf /var/lib/apt/lists/*
USER node
