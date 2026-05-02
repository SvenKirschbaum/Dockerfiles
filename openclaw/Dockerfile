FROM ghcr.io/openclaw/openclaw:2026.4.29@sha256:2c56a23cb2fe826a0d521bf6d61c8e4bb4e4201274f358eccddf5876393155ae
USER root
RUN apt-get update && apt-get install -y --no-install-recommends \
    ffmpeg \
    && rm -rf /var/lib/apt/lists/*
USER node
