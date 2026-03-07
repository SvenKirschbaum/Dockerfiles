FROM ghcr.io/openclaw/openclaw:2026.3.2@sha256:d60f848db7d5019336dfa17412588881a42774ad34a3429f03dd06d0a71c2848
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
