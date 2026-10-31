# Neural Mart — first repair pass

## Changes made

- Added an authenticated media-upload endpoint (`POST /api/uploads`) for up to 10 image/video files per post.
- Added file type and per-file size checks (200 MB), randomized server filenames, and a static `/uploads/` route.
- Rebuilt the Showcase Feed page with a working post composer, multi-file selection, local previews, remove-attachment controls, upload/publish states, and useful error messages.
- Added image/video rendering in the feed and connected the existing like/comment interaction component.
- Updated the home-page “Share your latest work” control to open the real composer.
- Replaced the dashboard's hard-coded profile-view count with real post, media-post, like and comment totals from the database. Profile views are explicitly not reported until actual view tracking is implemented.

## Setup

1. Install the workspace dependencies with `pnpm install` (this updates `pnpm-lock.yaml` for the added `multer` runtime dependency).
2. Configure the existing database and session environment variables as before.
3. Start the API and frontend using the project's existing Replit/workspace commands.
4. Ensure the API process has write access to the `uploads/` directory. Set `UPLOAD_DIR` to a persistent writable directory if needed.

## Deployment notes

- This version stores media on the API server's local filesystem. That is suitable for development or a single persistent server. For multi-instance or ephemeral deployments, configure durable object storage (such as S3/R2) before relying on uploaded files in production.
- Upload endpoint accepts JPG, PNG, WEBP, GIF, MP4, WEBM and MOV MIME types. Each file is limited to 200 MB and a post may contain up to 10 files.
- Like and comment totals use the existing database-backed interactions. Reach, impressions, watch time, and unique post views are not implemented in the current schema and are not fabricated by this patch.
- A full typecheck/build could not be run in the repair environment because workspace dependencies are not installed and the package registry was unreachable. Run `pnpm install` then `pnpm build` before deployment, and resolve any environment-specific build errors.
