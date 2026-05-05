# Shrine Agent Notes

Shrine is a long-lived fork of the open sourced Warp terminal. The goal is to stay close enough to upstream Warp that fixes can be brought in regularly, while keeping Shrine-specific changes small and easy to replay.

## Fork Workflow

- Keep `origin` pointed at the Shrine fork.
- Add the official Warp repository as `upstream`.
- Keep local Shrine changes as small commits on top of upstream, like a patch stack.
- Prefer syncing upstream in a throwaway branch before touching `master`.

## Sync Process

1. Fetch upstream.
2. Create a branch like `sync/upstream-YYYY-MM-DD`.
3. Merge upstream into that branch.
4. Resolve conflicts there, especially in Shrine-modified files.
5. Run focused tests.
6. Merge the sync branch back into `master` only after it looks good.

## Cherry-Picking

Cherry-pick only when a full upstream sync is intentionally being delayed.

Good cherry-pick candidates:

- Security fixes.
- Packaging or installer fixes.
- Small terminal behavior fixes.

Avoid cherry-picking large feature chains, internal-only changes, and cloud or agent work unless Shrine needs them directly.

## Current Shrine Patch Area

The known Shrine-specific editor text wrapping patch touches:

- `app/src/code/editor/model.rs`
- `app/src/code/editor/model_tests.rs`
- `app/src/code/editor/view.rs`
- `app/src/code/view.rs`

Expect upstream conflicts to be most likely around those files.

## Local Dev Run

Run the fork with an isolated local data profile:

```bash
WARP_DATA_PROFILE=shrine cargo run --features fast_dev
```

This keeps local Shrine data separate from the default `dev.warp.WarpOss` profile and enables `skip_login` for local development.

If Rust incremental compilation hits a local artifact error, rerun with:

```bash
WARP_DATA_PROFILE=shrine CARGO_INCREMENTAL=0 cargo run --features fast_dev
```
