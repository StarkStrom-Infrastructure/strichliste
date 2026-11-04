# strichliste image

Container image of [strichliste-backend](https://github.com/strichliste/strichliste-backend) (backend and web UI,
MIT), built from the upstream `Dockerfile` (stage `frankenphp_prod`: FrankenPHP, PHP 8.5, runs as `www-data`).
Upstream does not publish an image, so this repository builds one.

- Image: `ghcr.io/starkstrom-infrastructure/strichliste:<version>` (also `<major>.<minor>`), e.g. `2.1.2`.
  Deployments pin it by digest; Dependabot in the monorepo picks up new tags and digests.
- Upstream release: `upstream.env` (tag and commit). The `upstream` workflow opens a pull request when a new
  release appears; read its release notes before merging (migrations run at container start).
- `build` runs on every push to `main`, builds without pushing on pull requests, and rebuilds monthly for base
  image fixes.

## Running

Settings go into a mounted `config/strichliste.yaml` (see upstream `docs/Config.md`); the database is set with
`DATABASE_URL` (MariaDB/MySQL, PostgreSQL or SQLite), `APP_SECRET` must be set (`openssl rand -hex 32`).
Behind a reverse proxy set `SERVER_NAME=":80"` so FrankenPHP serves plain HTTP. The container applies database
migrations before it starts serving.

The deployment itself lives in the Infrastructure-Monorepo (`stacks/srv01/strichliste`).
