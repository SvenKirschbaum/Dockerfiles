FROM ghcr.io/openclaw/openclaw:2026.5.12@sha256:e2482a66682de6f540dcfd9921e410c23fd060dcd441382ff952247ee911a672
USER root
RUN apt-get update && apt-get install -y --no-install-recommends \
    ffmpeg \
    && rm -rf /var/lib/apt/lists/*
USER node
