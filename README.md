# pnpm `--no-bail` and dependents of a failed task

pnpm's documentation for `pnpm recursive` says:

> Even if `--no-bail` is used, all tasks will finish but if any of the tasks fail, the command will exit with a non-zero code.

This repository checks that sentence against real pnpm releases.

## Layout

- `lib` has a `test` script that fails.
- `app` depends on `lib` through `workspace:*` and has a `test` script that succeeds.
- `free` depends on nothing and has a `test` script that succeeds.

Each `test` script writes a marker file (`ran-lib`, `ran-app`, `ran-free`) in the repository root.
A marker exists only if that script actually ran.

## Reproduction

The workflow `.github/workflows/no-bail.yml` runs on a clean GitHub-hosted runner for each pnpm version.
Each pnpm release is downloaded from its GitHub release and checked against a pinned SHA-256 before use.
The workflow runs `pnpm -r --no-bail run test`, prints which of the three scripts ran, and fails when any did not.

A red job means the documented sentence is false for that pnpm version.
A green job means every task ran.

The results are in the Actions tab of this repository.
