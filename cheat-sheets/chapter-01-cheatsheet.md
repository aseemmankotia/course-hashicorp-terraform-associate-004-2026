## Chapter 1 Cheat Sheet: IaC Concepts (~8% of exam)

### Manual/Click-Ops Problems vs IaC Fix
| Problem with click-ops | How IaC fixes it |
|---|---|
| No change history / audit trail | Config stored in VCS (Git) — full history |
| Inconsistent environments | Same code applied repeatably = consistency |
| Undetected config drift | `terraform plan` compares real state vs config |
| No peer review before changes | Pull request workflow reviews code pre-apply |
| Slow, error-prone at scale | Automated, declarative provisioning |

### Declarative vs Imperative
| Aspect | Declarative (Terraform) | Imperative (scripts) |
|---|---|---|
| You specify | Desired end state ("what") | Exact steps in order ("how") |
| Execution order | Terraform determines it via dependency graph | You determine it manually |
| Idempotent by default | Yes | Not guaranteed — must code it yourself |
| Example | `resource "aws_instance" "web" {...}` | Sequential AWS CLI commands |

### Core IaC Benefits (memorize: V-R-D)
- **Versioning** — infra changes tracked in Git; rollbacks and diffs possible.
- **Repeatability** — same config → same result across dev/stage/prod.
- **Drift reduction/detection** — `terraform plan` surfaces divergence between real infra and code; `apply` reconciles it.

### Idempotency
- Applying the same config multiple times = same end result.
- Re-running `apply` with no code changes → "No changes. Infrastructure is up-to-date."

### Drift Detection Commands
| Command | Purpose | Makes changes? |
|---|---|---|
| `terraform plan` | Shows diff between state/config and real infra | No |
| `terraform apply` | Reconciles real infra to match config | Yes |
| `terraform validate` | Checks HCL syntax only | No (no state comparison) |
| `terraform destroy` | Removes managed resources | Yes |

### Multi-Cloud / Hybrid-Cloud Value
- Enabled by **provider plugins**: each provider (aws, azurerm, google, etc.) translates common HCL into that platform's API calls.
- **Multi-cloud**: 2+ public clouds managed with one tool/workflow.
- **Hybrid-cloud**: public cloud + on-prem/private infra managed together via multiple provider blocks.
- Exam framing: "single tool, consistent syntax, many platforms" → answer relates to provider architecture, not a nonexistent unified API.

### Common Exam Traps
- Confusing `terraform validate` (syntax only) with `terraform plan` (state/drift diff).
- Assuming IaC guarantees zero downtime — it does not; that depends on resource/architecture design.
- Thinking documentation or more manual process alone solves drift — only code + state comparison does.
- Believing Terraform silently ignores externally-made changes — it detects and reports drift on next plan.
