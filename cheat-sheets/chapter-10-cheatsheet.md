## Chapter 10 Cheat Sheet — Drift, Refactoring State & Import

### Version Introductions (memorize exact versions)
| Feature | Introduced In |
|---|---|
| moved block | 1.1 |
| -refresh-only plan/apply | 0.15.4 |
| import block (generate-config-out) | 1.5 |
| removed block | 1.7 |

### moved block vs removed block vs state rm vs state mv
| Tool | Purpose | Reviewable in plan? | Destroys infra? | Tracked in VCS? |
|---|---|---|---|---|
| moved block | Rename/relocate resource address | Yes | No | Yes |
| removed block (destroy=false) | Forget resource from state only | Yes | No | Yes |
| removed block (destroy=true, default) | Remove + destroy (same as deleting resource block) | Yes | Yes | Yes |
| terraform state rm | Immediate CLI state removal | No | No | No |
| terraform state mv | Immediate CLI rename/move in state | No | No | No |

**Exam trap:** removed block with NO lifecycle argument = destroy = true by default. Students often assume removed = always safe/state-only — wrong unless destroy = false is explicit.

### Drift Detection: -refresh-only
```bash
terraform plan -refresh-only    # preview drift only, no config changes proposed
terraform apply -refresh-only   # persist reconciled state, infra untouched
```
- Standard `terraform plan` also refreshes state internally, but bundles drift with proposed create/update/destroy actions toward config.
- `terraform refresh` (legacy) does the same state update as apply -refresh-only but skips the reviewable plan step — discouraged.

### moved block syntax
```hcl
moved {
  from = aws_instance.web
  to   = module.app.aws_instance.web
}
```
Only valid arguments: `from`, `to`.

### removed block syntax
```hcl
removed {
  from = aws_s3_bucket.shared
  lifecycle {
    destroy = false   # state-only removal; omit or set true to actually destroy
  }
}
```

### terraform import — requirements & limits
```bash
terraform import aws_instance.web i-0123456789abcdef0
```
- Requires a resource block already in config with matching type; import only writes to state (legacy CLI form does NOT generate config).
- Imports ONE resource per invocation (legacy CLI); does not cascade to related/child resources.
- Terraform 1.5+ `import` block supports batch imports and `-generate-config-out=generated.tf` to scaffold configuration.

### terraform state mv
```bash
terraform state mv aws_instance.old aws_instance.new
```
Imperative, immediate, not plan-reviewed — prefer moved block for anything committed to shared config.
