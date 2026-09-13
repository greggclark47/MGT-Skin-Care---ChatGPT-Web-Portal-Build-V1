# MGT-Skin-Care---ChatGPT-Web-Portal-Build-V2.5
MGT Skin Web Portal Build Repo

The browseable portal source is in `apps/`, `packages/`, and `infra/`. The ZIP files are dated recovery checkpoints; current code is available directly in this repository.

This is a pnpm workspace. From the repository root, install dependencies with `pnpm install --frozen-lockfile`, then run `pnpm -r build`. The API, web portal, AI gateway, local Ollama configuration, and PostgreSQL deployment files are part of the source tree. Copy `infra/portal/env.example` to a private environment file and configure production services outside source control.

The latest release controls and remaining validation work are described in `infra/portal/RELEASE-CHECKPOINT.md`. Passing local builds do not establish production readiness; live provider, database, backup restoration, and deployment checks are still required.
