# tailscale-terraform-resources

TOML resource specifications for Tailscale, consumed by [iac-forge][1] /
[pangea-forge][2] to generate code for every IaC platform (Pangea Ruby DSL,
Terraform, Pulumi, Crossplane, Ansible, Steampipe).

19 resources covering ACL, DNS, devices, OAuth clients, federated identity,
log streaming, posture integrations, tailnet keys, settings, webhooks.

```bash
iac-forge-cli generate --specs . --backend pangea --output-dir ../pangea-tailscale
```

See [CLAUDE.md](./CLAUDE.md) for the full spec schema and the provider auth
flow.

[1]: https://github.com/pleme-io/iac-forge
[2]: https://github.com/pleme-io/pangea-forge
