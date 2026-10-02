## Chapter 3 Cheat Sheet — Providers & State Basics

### required_providers block
```hcl
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.10"
    }
  }
}
```
Both `source` and `version` are needed per provider. `source` disambiguates registry namespace (e.g., `hashicorp/aws`).

### Version Constraint Operators
| Operator | Meaning | Example | Allows |
|---|---|---|---|
| `=` | exact | `= 5.10.0` | only 5.10.0 |
| `!=` | exclude | `!= 5.10.0` | anything but that version |
| `>`, `>=`, `<`, `<=` | range bound | `>= 4.0, < 5.0` | 4.x only |
| `~>` (2-part) | pessimistic, patch only | `~> 5.10` | >=5.10.0, <5.11.0 |
| `~>` (1-part after major) | pessimistic, minor+patch | `~> 5.0` | >=5.0.0, <6.0.0 |

### Provider Block & Aliases
```hcl
provider "aws" {
  region = "us-east-1"
}

provider "aws" {
  alias  = "secondary"
  region = "eu-west-1"
}

resource "aws_instance" "eu" {
  provider = aws.secondary
  # ...
}
```
Default provider block (no alias) is used automatically. Aliased blocks require explicit `provider = local_name.alias` in resources/modules.

### .terraform.lock.hcl
- Purpose: pins exact provider version + checksums for reproducible installs.
- **Commit to VCS** — unlike `.terraform/` directory (do NOT commit).
- Updates on: `terraform init -upgrade`, new provider added, or constraint change forcing new version.
- Plain `terraform init` reuses lock file if satisfied — no update.

### terraform init — Provider Installation
1. Reads `required_providers` for source + constraints.
2. Checks `TF_PLUGIN_CACHE_DIR` (if set) for cached binary — symlinks if found.
3. Otherwise downloads from registry (default `registry.terraform.io`).
4. Writes/validates `.terraform.lock.hcl`.

### State File Role
- Maps each `resource`/`data` block in config → real infra object (via ID + attributes).
- Enables `plan`/`apply` to compute diffs and detect drift.
- Stored per backend config (local `terraform.tfstate` by default); state file is NOT the same as the lock file or provider config.

### Quick Gotchas
- `~>` with more digits = narrower floating range (patch only) vs fewer digits (minor+patch).
- Lock file mismatch errors after teammate adds provider → run `terraform init -upgrade`, not `apply`.
- Provider aliases are declared in **provider blocks**, not in `required_providers`.
- Plugin cache is set via **env var** `TF_PLUGIN_CACHE_DIR`, not HCL syntax.
