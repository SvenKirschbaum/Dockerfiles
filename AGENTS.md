# Agent Notes

This repo contains standalone Docker image definitions, not an app workspace. There is no root build system, README, or package manifest; verify Dockerfile changes with focused `docker build` commands and workflow changes with `actionlint`.

## Images

- `keycloak/`: multi-stage Keycloak image pinned to `quay.io/keycloak/keycloak` by tag and digest. It downloads `argon2-password-hash-provider` with `ADD` from GitHub, sets `KC_DB=mariadb`, builds providers with `kc.sh build`, and starts with `kc.sh start --optimized`.
- `nginx-static/`: builds nginx from the upstream Git repo and checks out the latest `release-*` tag at build time, then fetches the latest `ngx-fancyindex` release from GitHub. Builds are network-dependent and not fully reproducible from only pinned base images.
- `teamcity-agent/`: extends `jetbrains/teamcity-agent:*linux-sudo`, installs Go, Node.js 24, zip, Make, and Amazon Corretto 25, sets Docker daemon MTU to 1400, then runs the TeamCity agent during image build against `https://dev.elite12.de/teamcity/` to pre-complete an agent upgrade before deleting logs and `buildAgent.properties`.

## Verification

- Build one image at a time from the repo root: `docker build -f keycloak/Dockerfile keycloak`, `docker build -f nginx-static/Dockerfile nginx-static`, or `docker build -f teamcity-agent/Dockerfile teamcity-agent`.
- Lint GitHub Actions workflows with `actionlint`.
- There are no repo-defined typecheck, unit test, or formatter commands.
- Expect `nginx-static` and `teamcity-agent` builds to need external network access; `teamcity-agent` also depends on the configured TeamCity server behavior during build.

## Automation

- Renovate config is `.github/renovate.json` and extends `github>fallobst22/renovate-config`.
- Renovate labels and branch prefixes are directory-specific for `keycloak`, `nginx-static`, and `teamcity-agent`; keep Dockerfile path changes aligned with those package rules.
- `jetbrains/teamcity-agent` uses custom regex versioning so tags like `2026.1.2-linux-sudo` are parsed correctly.
- `.github/mergify.yml` only extends `renovate-config`.
- GitHub Actions Docker builds use `.github/workflows/docker-image.yml` as the reusable build workflow, with one trigger workflow per image to preserve independent run numbers and path-filter builds to changed image directories.
- The reusable workflow builds images locally, runs Trivy in report-only mode against the local `vN` tag, then pushes on `main` only after the scan step completes.
- Pull requests build and scan matching images without pushing. Pushes to `main` publish to `registry.gitlab.com/fallobst22/images` using `GITLAB_REGISTRY_USERNAME` and `GITLAB_REGISTRY_PASSWORD` repository secrets.
- When adding a new image, add a small trigger workflow that calls `.github/workflows/docker-image.yml` instead of duplicating Docker build and push logic.
