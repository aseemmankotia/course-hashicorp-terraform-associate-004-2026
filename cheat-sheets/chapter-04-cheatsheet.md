## Chapter 4: Init, Validate, Plan

### terraform init — what it does
| Action | Detail |
|---|---|
| Provider download | Fetches provider plugins matching version constraints |
| Backend setup | Initializes/configures the state backend |
| Module install | Downloads referenced modules |
| Lock file | Writes `.terraform.lock.hcl` — **commit to VCS** |

### Key init flags
| Flag | Effect |
|---|---|
| `-backend=false` | Skip backend init (local-only testing) |
| `-reconfigure` | Change backend config, ignore existing state |
| `-migrate-state` | Change backend config AND migrate existing state |
| `-upgrade` | Upgrade providers/modules to latest allowed version |
| `-input=false` | Disable interactive prompts (CI use) |

### terraform validate
- Static, **offline** check: syntax + internal consistency (references, types)
- Does **not** contact providers, backend, or real infrastructure
- No credentials required
- Does NOT catch: drift, provider-side errors, permission issues

### terraform plan — output symbols
| Symbol | Meaning |
|---|---|
| `+` | Create |
| `-` | Destroy |
| `~` | Update in-place |
| `-/+` | Destroy then create (replace, default order) |
| `+/-` | Create then destroy (replace, `create_before_destroy = true`) |
| `(known after apply)` | Value not determinable until apply runs |

### validate vs plan
| | validate | plan |
|---|---|---|
| Contacts providers | No | Yes |
| Reads real infra | No | Yes (unless `-refresh=false`) |
| Requires credentials | No | Usually yes |
| Shows resource diff | No | Yes |

### Refresh behavior
- `terraform plan` refreshes state (reads real infra) by default
- `-refresh=false` — skip refresh, diff against last recorded state only
- `terraform plan -refresh-only` — update state to match real infra, no resource changes proposed
- Applying a **saved plan file** does NOT refresh again — executes exactly what was planned

### Saving and applying a plan
```
terraform plan -out=tfplan
terraform apply tfplan
```
- Saved plan applies with no interactive approval prompt (already reviewed)
- If state changed since save → apply **fails with 'stale plan' error**

### Speculative plans
- A plan with no path to apply: run without `-out`, or PR-triggered plan in HCP Terraform without apply permission
- Used for safe CI/PR review workflows

### Exam traps
- Confusing `-/+` (replace) with `~` (in-place update)
- Assuming validate catches provider/API errors — it doesn't
- Forgetting a stale saved plan errors out instead of silently re-planning
- Assuming applying a saved plan file re-refreshes state — it does not
