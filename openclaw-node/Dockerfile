FROM ghcr.io/openclaw/openclaw:2026.3.12@sha256:c871ddf7ad1b4125218e5f010b59724f0d2bb299714c5bd6ba7c1e99462450a7
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

#Workaround
RUN find /app/dist -type f -name '*.js' -exec sed -i '/function isSecureWebSocketUrl(url) {/a\    return true;' {} +


USER node
ENTRYPOINT ["node", "openclaw.mjs", "node", "run"]
