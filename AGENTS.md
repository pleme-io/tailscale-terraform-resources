# tailscale-terraform-resources — Agent Orientation

Pure TOML spec catalog for the Tailscale provider. No Ruby, no Rust, no Nix
beyond optional dev shell.

## Reading order

1. `CLAUDE.md` — full structure, usage, regeneration steps
2. `provider.toml` — provider auth surface
3. `resources/<category>/<resource>.toml` — one file per Terraform resource

## When you change a TOML

Re-run `iac-forge-cli generate --specs . --backend pangea --output-dir ../pangea-tailscale`
from this directory's parent. Diff the regenerated gem before committing.

## When upstream Tailscale provider releases a new version

Re-pull schemas via `iac-forge-cli sync --provider tailscale --version <v>` and
diff the resulting TOMLs. Keep edits surgical — most attribute changes are
1-line `description` updates.
