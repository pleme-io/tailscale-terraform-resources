# tailscale-terraform-resources — TOML resource specs for Tailscale

Declarative TOML specifications for the 19 Tailscale Terraform resources.
Same pattern as `datadog-terraform-resources` and `splunk-terraform-resources`.
Consumed by iac-forge / pangea-forge to generate code for every IaC platform.

## Structure

```
provider.toml                # Provider config (oauth_client_id/secret, api_key, tailnet, base_url)
resources/
  acl/
    acl.toml                                # tailscale_acl
  aws/
    aws_external_id.toml                    # tailscale_aws_external_id
  contacts/
    contacts.toml                           # tailscale_contacts
  devices/
    device_authorization.toml               # tailscale_device_authorization
    device_key.toml                         # tailscale_device_key
    device_subnet_routes.toml               # tailscale_device_subnet_routes
    device_tags.toml                        # tailscale_device_tags
  dns/
    dns_configuration.toml                  # tailscale_dns_configuration  (preferred)
    dns_nameservers.toml                    # tailscale_dns_nameservers    (legacy)
    dns_preferences.toml                    # tailscale_dns_preferences    (legacy)
    dns_search_paths.toml                   # tailscale_dns_search_paths   (legacy)
    dns_split_nameservers.toml              # tailscale_dns_split_nameservers (legacy)
  identity/
    federated_identity.toml                 # tailscale_federated_identity
  logstream/
    logstream_configuration.toml            # tailscale_logstream_configuration
  oauth/
    oauth_client.toml                       # tailscale_oauth_client
  posture/
    posture_integration.toml                # tailscale_posture_integration
  tailnet/
    tailnet_key.toml                        # tailscale_tailnet_key
    tailnet_settings.toml                   # tailscale_tailnet_settings
  webhooks/
    webhook.toml                            # tailscale_webhook
```

## Usage

```bash
# Generate Pangea Ruby resources (canonical path for pleme-io)
iac-forge-cli generate --specs . --backend pangea --output-dir ../pangea-tailscale

# Generate Terraform provider code (when we want to vendor a custom build)
iac-forge-cli generate --specs . --backend terraform

# All backends at once
iac-forge-cli generate --specs . --backend all
```

## Provider authentication

OAuth (preferred):

```sh
export TAILSCALE_OAUTH_CLIENT_ID=$(cat ~/.config/tailscale/oauth-client-id)
export TAILSCALE_OAUTH_CLIENT_SECRET=$(cat ~/.config/tailscale/oauth-client-secret)
```

API key (deprecated):

```sh
export TAILSCALE_API_KEY=tskey-api-…
```

Both auth flows are wired through `nix/profiles/darwin-developer/home/secrets.nix`
(SOPS-encrypted in the nix repo, rendered to `~/.config/tailscale/oauth-client-{id,secret}`).

## TOML spec format

Each `.toml` file defines one resource:

- `[resource]` — name, description, category
- `[crud]` — Tailscale REST API endpoints (create / read / update / delete)
- `[identity]` — id_field, import_field, force_new_fields
- `[fields.<name>]` — type, required, sensitive, computed, enum, description

Same schema as `datadog-terraform-resources` and `akeyless-terraform-resources`.
Nested blocks (e.g. tailscale_contacts.account, tailscale_dns_configuration.nameservers)
are encoded as JSON-string fields when their structure is irregular.

## Source of truth

Resource attributes were captured from the official tailscale/tailscale provider
v0.28.0 via the Terraform registry (terraform-plugin-docs). When the upstream
provider releases a new version, refresh by:

```bash
# Re-pull schemas and regenerate the TOMLs (semi-automatic — diff before committing)
iac-forge-cli sync --provider tailscale --version <new-version>
```

## See also

- `pangea-tailscale` — auto-generated Ruby DSL gem rendered from this catalog
- `pangea-forge` — the Ruby backend used to generate the gem
- `iac-forge` — the platform-independent IR
- `nix/profiles/darwin-developer/home/secrets.nix` — credential rendering
