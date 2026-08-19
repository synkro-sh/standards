# Synkro standards

The rule sets Synkro ships with. Each folder is one package: a
`synkro-package.toml` naming it, and a `rules/` directory of rule files.

Rules live in files, and only in files. A Synkro installation references this
repository from the user's `~/.synkro/synkro.toml` at a pinned revision, so what
is enforced on a machine is a commit you can read, diff, and review:

```toml
schema_version = 1

[dependencies]
cwe-baseline = { source = "git", url = "https://github.com/synkro-sh/standards.git", rev = "<commit>", path = "cwe-baseline" }
```

`synkro.lock` records what that pin resolved to, down to the digest of each rule
file, so a machine can prove which rules it is validating against.

## Packages

- **`cwe-baseline`** — the small, high-confidence set every installation gets by
  default: hardcoded credentials (CWE-798), dynamic code execution (CWE-94),
  weak cryptographic hashes (CWE-327), and disabled TLS verification (CWE-295).

## Adding or changing a rule

Every rule carries `[[examples]]` that are *executed* when the package loads:
at least two that must be caught, and at least one nearby case that must be
left alone. A rule whose examples do not behave as declared is rejected rather
than silently enforced, so the examples are the test, not documentation.

The rule file's shape is published at `schemas/policy/v1/synkro-rule.schema.json`
in [`synkro-sh/synkro-rs`](https://github.com/synkro-sh/synkro-rs).
