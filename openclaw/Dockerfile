FROM ghcr.io/openclaw/openclaw:2026.3.8@sha256:7b1294f6aa2eb05b2070cc614743f79212313fc294e5de221ada8a2969ea52f6
USER root
RUN apt-get update && apt-get install -y --no-install-recommends \
    ffmpeg \
    && rm -rf /var/lib/apt/lists/*
USER node
