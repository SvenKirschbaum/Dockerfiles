FROM ghcr.io/openclaw/openclaw:2026.3.24@sha256:7091859602df6b8cdd59b38adbaed723a6d94806fdd4274d488400dd2fcf0fb6
USER root
RUN apt-get update && apt-get install -y --no-install-recommends \
    ffmpeg \
    && rm -rf /var/lib/apt/lists/*
USER node
