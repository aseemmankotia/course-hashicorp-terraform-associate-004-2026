## Chapter 7 Cheatsheet — HCL Types, Loops, Validation, Sensitive Data

### count vs for_each
| Feature | count | for_each |
|---|---|---|
| Input | Number | Map or Set of strings |
| Addressing | resource[0], resource[1] | resource["key"] |
| Removing one item | Shifts indices → destroys/recreates later items | Only removes the matching key's resource |
| List input | Native | Requires toset(list) |
| Best for | Identical, order-independent copies | Items needing stable, independent identity |

### Complex Types
| Type | Syntax | Notes |
|---|---|---|
| list | list(type) | Ordered, all elements same type |
| set | set(type) | Unordered, unique, same type |
| map | map(type) | Key-value, all values same type |
| object | object({k=type,...}) | Named attrs, mixed types allowed |
| tuple | tuple([type1,type2]) | Ordered, fixed length, mixed types by position |

### Key Built-in Functions
| Function | Signature | Use |
|---|---|---|
| lookup | lookup(map, key, default) | Safe map read with fallback |
| merge | merge(map1, map2, ...) | Combine maps; later args override |
| join | join(sep, list) | List → string |
| split | split(sep, string) | String → list |
| contains | contains(list, value) | Membership check (used in validation) |
| coalesce | coalesce(v1, v2, ...) | First non-null value |

### dynamic Block
```hcl
dynamic "ingress" {
  for_each = var.ingress_rules
  content {
    from_port = ingress.value.port
    to_port   = ingress.value.port
    protocol  = "tcp"
    cidr_blocks = [ingress.value.cidr]
  }
}
```
Use for repeated nested blocks inside one resource. Custom iterator name via `iterator = "rule"`.

### Variable Validation
```hcl
variable "env" {
  type = string
  validation {
    condition     = contains(["dev","staging","prod"], var.env)
    error_message = "env must be dev, staging, or prod."
  }
}
```

### precondition / postcondition (Terraform 1.2+)
```hcl
lifecycle {
  precondition {
    condition     = var.instance_count > 0
    error_message = "instance_count must be positive."
  }
  postcondition {
    condition     = self.id != ""
    error_message = "resource id was not set."
  }
}
```
Goes inside resource, data, or output blocks only — NOT inside variable blocks.

### check Block (Terraform 1.5+)
```hcl
check "api_healthy" {
  assert {
    condition     = data.http.api.status_code == 200
    error_message = "API is not healthy."
  }
}
```
Warns on failure; does not block apply.

### Sensitive Data
- `sensitive = true` on variable/output → redacts value from CLI/plan/apply output only.
- Value is **still stored in plaintext in state** — protect state via encrypted backend + access control.
- Vault provider: fetches secrets dynamically at runtime (e.g. `data.vault_generic_secret`), reducing hardcoded secrets in config, but does not remove the state-protection requirement.

### Common Exam Traps
- Choosing count when resource list can shrink from the middle → causes cascading recreates.
- Passing a list directly to for_each without toset().
- Assuming sensitive = true encrypts state — it does not.
- Putting precondition inside a variable block instead of lifecycle in resource/output.
