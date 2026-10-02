## Chapter 8 Cheat Sheet: Terraform Modules

### Module Source Formats

| Source Type | Format | Version Arg Support |
|---|---|---|
| Local path | `./modules/x` or `../modules/x` | ❌ Not allowed — errors if used |
| Public Registry | `NAMESPACE/NAME/PROVIDER` (3 parts) | ✅ Yes |
| Private Registry | `HOSTNAME/NAMESPACE/NAME/PROVIDER` (4 parts) | ✅ Yes |
| Git (generic) | `git::https://host/repo.git?ref=TAG` | ❌ Use `?ref=` instead |
| GitHub shorthand | `github.com/user/repo?ref=TAG` | ❌ Use `?ref=` instead |

**Exam trap:** Only Registry sources (public or private) support the `version` argument. Local and Git sources must use `?ref=` or no pinning at all.

### Module Block Syntax

```hcl
module "vpc" {
  source  = "terraform-aws-modules/vpc/aws"
  version = "~> 5.0"

  name = "my-vpc"
  cidr = "10.0.0.0/16"
}
```

- Label after `module` (here `vpc`) is the local reference name — used in `module.vpc.*`
- Arguments other than `source`/`version`/meta-args (`count`, `for_each`, `providers`, `depends_on`) map to the module's declared input variables

### Version Constraint Operators

| Constraint | Meaning |
|---|---|
| `= 1.2.0` | Exactly 1.2.0 |
| `>= 1.2.0` | 1.2.0 or newer |
| `<= 2.0.0` | 2.0.0 or older |
| `~> 1.2` | `>= 1.2.0, < 2.0.0` (major pinned) |
| `~> 1.2.0` | `>= 1.2.0, < 1.3.0` (major+minor pinned) |

**Exam trap:** The number of version segments in `~>` determines what's pinned — fewer segments = wider allowed range.

### Referencing Outputs & Passing Variables

```hcl
# Child module output
output "vpc_id" {
  value = aws_vpc.this.id
}

# Root module reference
resource "aws_subnet" "example" {
  vpc_id = module.vpc.vpc_id
}
```

- Reference pattern: `module.<NAME>.<OUTPUT>`
- Variables passed in via module block arguments; **no automatic inheritance** by name between root/child
- Each module has an **isolated variable scope** — required variables with no default and no assignment cause plan/apply to fail or prompt

### Nesting Modules

- A module can itself call other modules (nested/composed modules)
- Outputs must be explicitly re-exposed at each level to reach the root — no automatic pass-through up the chain
- `terraform init` / `terraform get` downloads all nested module sources recursively

### Quick Command Reference

| Command | Purpose |
|---|---|
| `terraform init` | Downloads modules + providers, sets up backend |
| `terraform get` | Downloads/updates only modules (no provider/backend init) |
| `terraform get -update` | Re-downloads modules even if already present |
