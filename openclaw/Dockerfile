FROM ghcr.io/openclaw/openclaw:2026.4.10@sha256:1486d55ecb1b35a0e3167f02324958566007dfdc505950f87402c01bf637885f
USER root
RUN apt-get update && apt-get install -y --no-install-recommends \
    ffmpeg \
    && rm -rf /var/lib/apt/lists/*
USER node
