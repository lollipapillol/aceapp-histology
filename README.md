# Aceapp Histology

Separate Aceapp subject repository for visual-first Histology laboratory training.

## Current base

Aceapp Histology v13 — Precision Gap Closure.

The current `index.html` is the working local build and is suitable as the repository
starting point.

## Backend recommendation

Reuse the existing Aceapp Anatomy Firebase project (`aceapp-70837`) so Anatomy and
Histology share one account system.

Histology progress should be isolated at:

`users/{uid}/subjects/histology`

See `FIREBASE_SETUP.md` and `firestore.rules`.

## Deployment

Create a separate Netlify site connected only to this repository.

The included `netlify.toml` publishes the repository root.

## Important image note

The current development build loads many real micrographs through their verified
source URLs. For a production release, approved assets should eventually be stored
locally/object storage rather than permanently hotlinked.
