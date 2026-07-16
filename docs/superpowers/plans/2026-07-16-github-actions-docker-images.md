# GitHub Actions Docker Images Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Migrate the existing TeamCity Docker image builds to GitHub Actions while preserving per-image build behavior and PR verification.

**Architecture:** Use one reusable workflow for Docker build, report-only Trivy scanning, and push logic, plus one small trigger workflow per image. The trigger workflows keep independent run numbers, path-filter builds to changed image directories, and pass image-specific metadata to the reusable workflow.

**Tech Stack:** GitHub Actions, Docker Buildx, `docker/login-action`, `docker/build-push-action`, `aquasecurity/trivy-action`, GitLab Container Registry.

## Global Constraints

- Preserve TeamCity registry target: `registry.gitlab.com/fallobst22/images`.
- Preserve TeamCity build context checkout behavior by using each image directory as the Docker build context.
- Preserve TeamCity `--pull` behavior with Docker Buildx `pull: true`.
- Preserve TeamCity push behavior: `keycloak` pushes only `vN`; `nginx-static` and `teamcity-agent` push `vN` and `latest`.
- Preserve independent per-image numbering with one trigger workflow per image and offsets from TeamCity counters: `keycloak` offset `66`, `nginx-static` offset `35`, `teamcity-agent` offset `48`.
- PR builds must verify Docker builds without pushing images.
- All builds must run Trivy against the locally built `vN` image tag in report-only mode before any `main` push.
- Workflows must run only for image directories or workflow files that actually changed.
- Required repository secrets: `GITLAB_REGISTRY_USERNAME` and `GITLAB_REGISTRY_PASSWORD`.
- Do not commit changes unless the user explicitly asks for a commit.

---

### Task 1: Reusable Docker Build Workflow

**Files:**
- Create: `.github/workflows/docker-image.yml`

**Interfaces:**
- Consumes: `workflow_call` inputs `image`, `context`, `version_offset`, `push_latest` and secrets `GITLAB_REGISTRY_USERNAME`, `GITLAB_REGISTRY_PASSWORD`.
- Produces: A reusable job that trigger workflows can call to build and scan on PRs and build, scan, and push on `main`.

- [x] **Step 1: Create the reusable workflow**

Create `.github/workflows/docker-image.yml` with a `workflow_call` interface, tag calculation, Docker Buildx local build, report-only Trivy scan, Docker registry login, and Docker image push.

- [x] **Step 2: Verify workflow syntax locally where possible**

Run: `git diff --check`
Expected: no whitespace errors.

### Task 2: Per-Image Trigger Workflows

**Files:**
- Create: `.github/workflows/keycloak.yml`
- Create: `.github/workflows/nginx-static.yml`
- Create: `.github/workflows/teamcity-agent.yml`

**Interfaces:**
- Consumes: `.github/workflows/docker-image.yml` reusable workflow.
- Produces: Image-specific workflows with independent run numbers, path filters, PR build/scan verification, and push-on-main publishing.

- [x] **Step 1: Add `keycloak` workflow**

Create `.github/workflows/keycloak.yml` that triggers on `pull_request`, `push` to `main`, and `workflow_dispatch`, path-filtered to `keycloak/**`, its own workflow, and the reusable workflow. Pass `image: keycloak`, `context: keycloak`, `version_offset: 66`, and `push_latest: false`.

- [x] **Step 2: Add `nginx-static` workflow**

Create `.github/workflows/nginx-static.yml` with equivalent triggers and pass `image: nginx-static`, `context: nginx-static`, `version_offset: 35`, and `push_latest: true`.

- [x] **Step 3: Add `teamcity-agent` workflow**

Create `.github/workflows/teamcity-agent.yml` with equivalent triggers and pass `image: teamcity-agent`, `context: teamcity-agent`, `version_offset: 48`, and `push_latest: true`.

- [x] **Step 4: Verify path-filtered trigger coverage**

Read each trigger workflow and confirm each includes its image directory, its own workflow file, and `.github/workflows/docker-image.yml` in both `pull_request.paths` and `push.paths`.

### Task 3: Final Verification

**Files:**
- Verify: `.github/workflows/*.yml`

**Interfaces:**
- Consumes: all workflows created in Tasks 1 and 2.
- Produces: Verified CI migration ready for user review.

- [x] **Step 1: Run whitespace validation**

Run: `git diff --check`
Expected: no output and exit code `0`.

- [x] **Step 2: Inspect final diff**

Run: `git diff -- .github/workflows docs/superpowers/plans`
Expected: only the reusable workflow, three trigger workflows, and this plan are changed.

- [x] **Step 3: Report required secrets and verification limitations**

Report that live GitHub Actions publishing requires repository secrets `GITLAB_REGISTRY_USERNAME` and `GITLAB_REGISTRY_PASSWORD`; Trivy runs in report-only mode and local Docker builds are not run unless requested because `nginx-static`/`teamcity-agent` are network-dependent.

## Self-Review

- Spec coverage: The plan covers TeamCity build contexts, registry, tag behavior, independent counters, PR verification, report-only Trivy scanning, path-filtered builds, and reusable workflow extensibility.
- Placeholder scan: No placeholders remain.
- Type consistency: Workflow inputs are consistently named `image`, `context`, `version_offset`, and `push_latest`.
