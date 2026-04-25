FROM ghcr.io/openclaw/openclaw:2026.4.23@sha256:1e6e1b562bbaa4816005f7a379fe2f5bc894289a08c0631f64263d705b81d809
USER root
RUN apt-get update && apt-get install -y --no-install-recommends \
    ffmpeg \
    && rm -rf /var/lib/apt/lists/*
USER node
