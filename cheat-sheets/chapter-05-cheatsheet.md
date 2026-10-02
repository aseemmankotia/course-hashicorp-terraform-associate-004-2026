## Chapter 5 Cheat Sheet: Apply, Destroy, Fmt & Graph

### terraform apply -- Two Modes
| Mode | Command | Confirmation Prompt? |
|---|---|---|
| Inline (generates plan then applies) | `terraform apply` | Yes -- must type `yes` |
| Saved plan (pre-approved) | `terraform apply tfplan` | No -- applies exactly what was saved |
| Either mode, skip prompt | add `-auto-approve` | Skips prompt (redundant on saved-plan mode) |

**Exam note:** `-auto-approve` in CI on an *inline* apply with no prior review = highest risk pattern tested. Safer pipeline pattern: `plan -out=file` → manual/automated review → `apply file` (no `-auto-approve` needed).

### terraform destroy
| Command | Scope |
|---|---|
| `terraform destroy` | Destroys ALL resources in current state |
| `terraform destroy -target=<addr>` | Destroys only named resource + its dependents |

**Caution:** `-target` is documented by HashiCorp for exceptional/recovery use only -- routine use risks state/infrastructure drift because dependency accounting is partial. Still prompts for `yes` unless `-auto-approve` is added.

### terraform fmt vs terraform validate
| Command | Checks | Does NOT check |
|---|---|---|
| `terraform fmt` | Canonical style: indentation, spacing, alignment | Logic, syntax errors, provider schema |
| `terraform validate` | Syntax + internal consistency (required args, types) | Style/formatting, real infra state |

**Memory trick:** fmt = *format*, validate = *verify*.

### terraform graph
- Outputs the resource **dependency graph** in **DOT format**.
- Pipe to Graphviz to render: `terraform graph | dot -Tsvg > graph.svg`
- Reflects the actual order Terraform will use to create/update/destroy resources.

### Parallelism
| Setting | Value |
|---|---|
| Default concurrent operations during apply/destroy | **10** |
| Change via | `-parallelism=n` flag on plan/apply/destroy |

**Exam note:** Parallelism controls *how many* independent operations run at once, not the *order* -- order is always dictated by the dependency graph.

### Common Traps
1. Thinking `-auto-approve` is required to apply a saved plan file (it isn't -- saved plans never prompt).
2. Assuming `terraform fmt` catches configuration errors (it only formats).
3. Using `-target` as a routine workflow shortcut instead of an exceptional recovery tool.
4. Confusing `terraform graph`'s DOT text output with a rendered image (Graphviz required).
