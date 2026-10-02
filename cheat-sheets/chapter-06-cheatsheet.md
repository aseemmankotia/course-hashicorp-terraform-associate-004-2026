# HCL: Resources, Data Sources & Dependencies

## Resource vs Data Block
| Aspect | resource | data |
|---|---|---|
| Manages lifecycle? | Yes (create/update/destroy) | No — read-only |
| Refresh timing | On apply when config changes | Every plan/apply |
| Syntax | `resource "type" "name" {}` | `data "type" "name" {}` |
| Reference | `type.name.attr` | `data.type.name.attr` |
| Bring under Terraform | `terraform import` | N/A (can't own resources) |

## Dependencies
| Type | Trigger | Syntax |
|---|---|---|
| Implicit | Attribute reference in expression | `subnet_id = aws_subnet.main.id` |
| Explicit | Hidden/behavioral dependency, no attribute link | `depends_on = [aws_iam_role_policy.x]` |

**Rule:** Use implicit references whenever possible. Use `depends_on` only when Terraform truly cannot infer the order (e.g., IAM eventual consistency, side effects not expressed via attributes).

**depends_on gotchas:**
- Must be a **list**: `depends_on = [resource_type.name]`
- References the **resource itself**, not an attribute (`.id` is wrong)
- Works on resources, data sources, and modules

## Destroy Order
Terraform destroys in **reverse** of the dependency graph: dependents removed before their dependencies.

## variable Block
```hcl
variable "instance_count" {
  type        = number
  default     = 2
  description = "Number of instances to create"
  sensitive   = false
  validation {
    condition     = var.instance_count > 0
    error_message = "instance_count must be greater than 0."
  }
}
```
- `type` — recommended, catches bad input early
- `default` — omit to make the variable required
- `validation` — plan-time custom constraint check
- `sensitive` — hides value in CLI output only (still visible in state file)

## output Block
```hcl
output "db_password" {
  value       = aws_db_instance.main.password
  description = "Generated DB password"
  sensitive   = true
}
```
- `sensitive = true` (boolean, not string) suppresses console display
- Does **not** encrypt state — state still stores plaintext value

## Common Exam Traps
1. Assuming a `data` block can create/manage a resource — it cannot.
2. Using `depends_on` with `.id`/`.arn` instead of the bare resource reference.
3. Forgetting `sensitive` doesn't protect the state file, only CLI output.
4. Believing destroy order is unrelated to dependency graph — it's always reverse order.
5. Adding `depends_on` everywhere 'to be safe' instead of relying on implicit references — exam rewards knowing when it's actually needed.
