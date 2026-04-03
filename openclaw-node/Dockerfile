FROM ghcr.io/openclaw/openclaw:2026.4.2@sha256:0b2170d5ec3a487a6313ed0556d377c5c5c80a0f806043daa2e685a4bedd45e3
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
