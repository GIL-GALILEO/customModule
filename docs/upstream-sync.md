# Upstream Sync

## Goal

Keep `GIL-GALILEO/customModule` close to `ExLibrisGroup/customModule` so Galileo-specific work stays in the wrapper repo whenever possible.

## Included Workflow

This fork includes:

- `.github/workflows/sync-upstream.yml`

The workflow:

- runs manually with `workflow_dispatch`
- also runs weekly on Monday
- fetches `ExLibrisGroup/customModule`
- attempts a fast-forward update of `main`
- pushes the result back to `origin`

## Important Constraint

The workflow uses `git merge --ff-only upstream/main`.

That is intentional:

- if the fork has no extra commits on `main`, sync is automatic
- if `main` has diverged, the workflow fails instead of creating a messy merge commit

## Recommended Practice

- keep `main` as close to upstream as possible
- put Galileo-specific orchestration in `primo-nde-devenv`
- use short-lived branches for fork-only fixes
- merge fork-only fixes carefully and only when they truly belong in the starter repo

## Branch Protection Note

If branch protection blocks GitHub Actions from pushing to `main`, either:

- allow GitHub Actions to bypass that protection for this repository
- or run the workflow as a visibility check and complete the sync manually

## Manual Recovery

If the workflow fails because the fork has diverged:

```bash
git fetch upstream
git checkout main
git merge --ff-only upstream/main
```

If fast-forward is not possible, review the local fork commits and decide whether they should:

- move to `primo-nde-devenv`
- become a small maintained fork patch
- or be dropped in favor of upstream behavior
