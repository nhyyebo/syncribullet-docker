# syncribullet-docker

Automated Docker image builder for [`aliyss/syncribullet`](https://github.com/aliyss/syncribullet). This repository uses GitHub Actions to pull the upstream repo, build a production image, and publish it to Docker Hub.

## How it works
- Checks out `aliyss/syncribullet` into `./upstream` during the workflow.
- Writes a simple multi-stage `Dockerfile` that:
  - Builds the project with Node 20 Alpine (`npm ci && npm run build`).
  - Serves the built `dist/` via `nginx:alpine` on port 80.
- Pushes multi-arch images (amd64, arm64) to Docker Hub.

## Prerequisites
Add the following repository secrets (Settings → Secrets and variables → Actions):
- `DOCKERHUB_USERNAME`: your Docker Hub username
- `DOCKERHUB_TOKEN`: a Docker Hub access token

The image will be published as:
```
$DOCKERHUB_USERNAME/syncribullet:latest
$DOCKERHUB_USERNAME/syncribullet:upstream-<shortsha>
```

## Triggers
- Manual: use "Run workflow"
- Scheduled: daily at 06:00 UTC
- On push to `main` in this wrapper repo

## Notes
- This assumes `aliyss/syncribullet` builds to `dist/` (Vite/Qwik). If upstream changes to a server runtime, adjust the Dockerfile step in `.github/workflows/build-and-publish.yml` accordingly.
- You can add more tags or platforms in the workflow as needed.
