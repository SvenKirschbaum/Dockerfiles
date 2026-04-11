FROM ghcr.io/openclaw/openclaw:2026.4.9@sha256:d65cc3d5fd0c8b1f752c2f70377843230112250c10e99c3b61769234c217c5db
USER root
RUN apt-get update && apt-get install -y --no-install-recommends \
    ffmpeg \
    && rm -rf /var/lib/apt/lists/*
USER node
