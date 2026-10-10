FROM nousresearch/hermes-agent:v2026.9.24@sha256:fca358f12efd65bfaaca05884166f15c0e2788375ca30d77061ac1ebc96452b7
USER root
RUN apt-get update && apt-get install -y --no-install-recommends \
    ffmpeg \
    python3-docx \
    python3-openpyxl \
    python3-debugpy \
    python3-pypdf \
    python3-reportlab \
    python3-googleapi \
    python3-google-auth \
    python3-google-auth-oauthlib \
    gh \
    libopus0 \
    libsndfile1 \
    && rm -rf /var/lib/apt/lists/*

RUN npm install -g --allow-scripts=@googleworkspace/cli,@opencode/cli @opencode/cli @googleworkspace/cli

RUN uv pip install -U piper-tts

USER hermes
