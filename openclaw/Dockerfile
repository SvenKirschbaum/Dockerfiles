FROM ghcr.io/openclaw/openclaw:2026.2.14@sha256:ace6f32961c4d574cb189d0007ec778408a9c02502f38af9ded6c864bae0f454
USER root
RUN apt-get update && apt-get install -y --no-install-recommends \
    ffmpeg \
    && rm -rf /var/lib/apt/lists/*
USER node
