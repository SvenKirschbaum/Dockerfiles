FROM ghcr.io/openclaw/openclaw:2026.2.19@sha256:5352d3ababbc12237fda60fe00a25237441eb7bb5e3d3062a6b0b5fbd938734d
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
