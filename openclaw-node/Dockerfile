FROM ghcr.io/openclaw/openclaw:2026.5.7@sha256:1af3f457a2d5a1d210f4d95634fa5da6e23f9c0ac7b52ef4bc38e2ecf09704fd
USER root
RUN apt-get update && apt-get install -y --no-install-recommends \
    ffmpeg \
    gh \
    git \
    jq \
    yq \
    && rm -rf /var/lib/apt/lists/*
RUN npm i -g opencode-ai
RUN curl -L -o - https://github.com/steipete/goplaces/releases/download/v0.3.0/goplaces_0.3.0_linux_amd64.tar.gz | tar -xzvC /usr/local/bin && chmod +x /usr/local/bin/goplaces
RUN curl -L -o /usr/local/bin/kubectl "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl" && chmod +x /usr/local/bin/kubectl


USER node
ENTRYPOINT ["node", "openclaw.mjs", "node", "run"]
