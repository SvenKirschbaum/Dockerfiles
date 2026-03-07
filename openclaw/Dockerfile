FROM ghcr.io/openclaw/openclaw:2026.3.2@sha256:d60f848db7d5019336dfa17412588881a42774ad34a3429f03dd06d0a71c2848
USER root
RUN apt-get update && apt-get install -y --no-install-recommends \
    ffmpeg \
    && rm -rf /var/lib/apt/lists/*
USER node
