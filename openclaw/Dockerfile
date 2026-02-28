FROM ghcr.io/openclaw/openclaw:2026.2.26@sha256:ce9347548afa0b6bdd1d262060535ba04baf0b19cde0fc211c8039492647d1b1
USER root
RUN apt-get update && apt-get install -y --no-install-recommends \
    ffmpeg \
    && rm -rf /var/lib/apt/lists/*
USER node
