# Whimsy Little update channel

This repository is the public delivery channel for Whimsy Little HTML updates.

## Release layout

- `manifest.json` — the only mutable production pointer.
- `releases/<version>/parts/` — immutable update payloads. Never edit or reuse a published version folder.
- Legacy root `parts/` files are retained only for compatibility with the first updater bootstrap.

## Release rules

1. Build and smoke-test the new Whimsy HTML.
2. Generate a compact patch against the frozen HTML base bundled in the Android shell.
3. Upload the patch to a new immutable `releases/<version>/parts/` folder.
4. Verify every uploaded part.
5. Create the unsigned manifest request in the private Android repository.
6. Sign it there with the permanent Whimsy Android release key.
7. Publish the signed `manifest.json` here **last**.

The Android shell verifies the manifest signature, update hashes, reconstructed HTML version, and startup health before the update is considered good.

## Staged rollout

`rolloutPercentage` can be moved through values such as 10 → 25 → 50 → 100. Each change requires a newly signed manifest. Devices are assigned deterministically to a cohort, so increasing the percentage only adds devices; it does not reshuffle existing ones.

## Rollback

A startup failure automatically rolls back locally to the built-in Whimsy bundle. If a release is healthy enough to confirm but later needs to be withdrawn, ship a new higher version containing the last known-good app rather than mutating or downgrading an existing release folder.

No signing secrets belong in this public repository.
