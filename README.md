# ruff

> [!WARNING]
> This module is deprecated, use [dagger/python](https://github.com/dagger/python)/ruff instead.
>
> Install it with `dagger module install dagger.io/python/ruff`

A [Dagger](https://dagger.io) module — written in the `.dang` module language —
that wraps [ruff](https://docs.astral.sh/ruff/), the extremely fast Python
linter and formatter.

Ruff ships as a self-contained static binary, so this module extracts it from
the pinned `ghcr.io/astral-sh/ruff` image (tracked by Dependabot via
[`images/ruff/Dockerfile`](images/ruff/Dockerfile)) and runs it on an Alpine
base — no Python, `pip`, `uv`, or virtualenv required.

## Functions

| Function       | Description                                                             |
| -------------- | ----------------------------------------------------------------------- |
| `check`        | Lint with `ruff check`; fails on violations (a `@check`).               |
| `format-check` | Verify formatting with `ruff format --check`; fails if unformatted.     |
| `lint`         | Return the lint report as text without failing on violations.           |
| `fix`          | Apply `ruff check --fix` and return the modified source directory.      |
| `format`       | Run `ruff format` and return the formatted source directory.            |
| `version`      | The version of the bundled ruff binary.                                  |

The constructor takes the project `source` from the workspace at `sourcePath`
(default `/`). `args` are forwarded to every ruff invocation, and `container`
overrides the default Alpine + ruff base.

## Usage

Run against the current workspace with the beta.6 engine:

```sh
dagger --x-release=v1.0.0-beta.6 -m . -c 'check'
dagger --x-release=v1.0.0-beta.6 -m . -c 'lint'
dagger --x-release=v1.0.0-beta.6 -m . -c 'fix | export .'
```

Scope to a subdirectory and pass ruff options via `args`:

```sh
dagger --x-release=v1.0.0-beta.6 -m . -c 'ruff --source-path src --args "--select,E,F" | check'
```
