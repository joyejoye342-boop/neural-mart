# Neural Mart — Render deployment notes

## What this update changes
- Vite no longer requires `PORT` and `BASE_PATH` to exist just to create a production build. Defaults are `5000` and `/`.
- The API server serves the built React frontend in production when `artifacts/neuralmart/dist/public` exists, so frontend `/api` requests remain same-origin.
- `render.yaml` documents a Render web service and a PostgreSQL service. Review the database service settings and pricing/availability in your Render account before deploying.

## Required checks before launch
1. Commit the complete repository folder, not the ZIP file contents nested inside another project folder.
2. Deploy using the repository root, where `pnpm-workspace.yaml` and `package.json` live.
3. Confirm the PostgreSQL service is available and `DATABASE_URL` is set.
4. `SESSION_SECRET` must be a strong secret; the blueprint generates one.
5. File uploads currently use local disk (`uploads/`). On ephemeral hosting disks, uploaded images/videos may disappear on redeploy or restart. Use durable object storage (such as S3/R2) before relying on uploads in production.
6. Verify login, create-post, image/video upload, likes and comments after deployment.

This configuration is a deployment starting point, not a guarantee that every feature has passed a live production test.
