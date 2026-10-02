## Chapter 2 Cheat Sheet: Terraform vs Other IaC & Config Tools

### Declarative vs Procedural
| Approach | Description | Example Tools |
|---|---|---|
| Declarative | Specify desired end state; tool computes steps | Terraform, CloudFormation |
| Procedural/Imperative | Specify exact ordered steps to reach outcome | Shell scripts, some Chef recipes |

Exam note: Terraform = declarative. It never asks 'how', only 'what'.

### Provisioning vs Configuration Management
| Category | Purpose | Model | Examples |
|---|---|---|---|
| Provisioning | Create/update/destroy infra resources | State-driven | Terraform, CloudFormation |
| Configuration Management | Configure OS/app on running nodes | Agent-based or agentless | Chef/Puppet (agent-based), Ansible (agentless) |

Common pattern: **Terraform provisions → Ansible configures**.

### Key Commands
| Command | Purpose |
|---|---|
| `terraform plan` | Preview changes (no side effects) |
| `terraform plan -out=FILE` | Save plan for later exact apply |
| `terraform apply FILE` | Apply a previously saved plan |
| `terraform graph` | Output DOT-format resource dependency graph |
| `terraform validate` | Check syntax/internal consistency only |

### The Resource Graph
- Terraform builds a DAG (directed acyclic graph) from implicit (attribute reference) and explicit (`depends_on`) dependencies.
- Independent resources are created in parallel; dependent resources are sequenced.
- `terraform graph` visualizes this (render with Graphviz).

### Execution Plan (`terraform plan`)
- Diffs desired config (.tf files) against current state file.
- Shows `+` create, `~` update in-place, `-/+` replace, `-` destroy.
- Zero-impact dry run — mandatory review step before `apply` in production workflows.

### Terraform Registry
- Hosts **providers** (plugins translating HCL → API calls) and **modules** (reusable configs).
- Contains official (HashiCorp), verified (partner), and community modules.
- Not to be confused with Ansible Galaxy (Ansible roles/collections only).

### Terraform + Ansible Positioning
| Concern | Terraform | Ansible |
|---|---|---|
| Primary job | Infra lifecycle/provisioning | In-instance configuration |
| State tracking | Yes (state file) | No persistent state file |
| Agent required | No | No (agentless via SSH/WinRM) |
| Idempotency | Native via state diffing | Native via module design |

Exam trap: Don't pick Ansible as the answer for 'provision cloud resources with dependency-aware ordering' — that's Terraform's job. Don't pick Terraform's `remote-exec` for ongoing OS configuration — hand that to Ansible.
