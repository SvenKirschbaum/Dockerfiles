FROM ghcr.io/openclaw/openclaw:2026.2.19@sha256:5352d3ababbc12237fda60fe00a25237441eb7bb5e3d3062a6b0b5fbd938734d
USER root
RUN apt-get update && apt-get install -y --no-install-recommends \
    ffmpeg \
    && rm -rf /var/lib/apt/lists/*
USER node
