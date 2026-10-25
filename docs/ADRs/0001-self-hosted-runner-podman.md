# 0001-self-hosted-runner-podman

## Status
Proposed

## Context
GitHub Actions workflows can declare `runs-on: self-hosted` to use a runner on your own machine instead of GitHub's cloud runners. The goal is to have CI jobs execute inside Podman containers on a local development machine (uid 1000, rootless). The official `actions/runner` binary speaks the Docker API, and Podman provides a Docker-compatible API socket at `/run/user/1000/podman/podman.sock`. This means the runner does not need Docker installed — it talks to Podman directly.

Key constraints and questions:
- The machine runs rootless Podman (user jpfeng, uid 1000).
- The runner must be able to create job containers and service containers (via `services:` in the workflow) through the Podman socket.
- The runner process needs to keep a persistent HTTPS long-poll connection to GitHub to receive jobs as they are queued.
- Security: self-hosted runners must not be exposed to public fork PRs without protection, as arbitrary code from PRs runs on the host machine.
- The setup should be repo-level (not org-level) for the initial iteration.

## Decision
Deploy the official `actions/runner` v2.338.1 as a long-running process on the local machine (started via `systemd --user` unit or manually via `./run.sh`). The runner registers with the target GitHub repo at repo-level, using labels `self-hosted, podman`. The `.env` file supplied to the runner sets `DOCKER_HOST=unix:///run/user/1000/podman/podman.sock`, causing the runner's Docker API client to use Podman's socket. Job containers (e.g., `docker.io/library/alpine:3.20`) and service containers (e.g., postgres) are created by the runner through the Podman API, resulting in real Podman containers on the host. The runner keeps an open HTTPS long-poll connection to GitHub; when a job is queued matching the runner's labels, GitHub pushes the job message over that connection within seconds. The runner then spawns a worker that executes the workflow steps inside the job container, with all container lifecycle managed through the Podman socket.

## Consequences
- **Pros**: No Docker needed on the host; leverages existing rootless Podman setup; jobs run in real containers (visible via `podman ps`); long-poll connection gives near-instant dispatch (seconds) when a workflow is triggered; repo-level registration is simple and has a clean security model (private repo only).
- **Cons**: One resident process (or systemd unit) stays active when no jobs are running; the runner process must have read/write access to the Podman socket; image accumulation in Podman requires periodic `podman image prune`; fork PRs on a public repo are unsafe (arbitrary code execution on the host).
- **Related decisions**: Phase 2 could containerize the runner itself inside Podman; Phase 3 could add ephemeral/wake-on-demand runners using `workflow_job` webhooks.

## Tags
github-actions, self-hosted-runner, podman, docker-api, ci-cd