# Chapter 9 Cheat Sheet: Terraform State & Remote Backends

## Why State Exists
| Question | Answer |
|---|---|
| Purpose | Maps config resources → real-world object IDs + attributes |
| Used for | Computing diffs on next `plan`, avoiding re-creating resources |
| Format | JSON document, plaintext (including sensitive values) |

## Local Backend (Default)
| Item | Detail |
|---|---|
| Default file | `terraform.tfstate` in working dir |
| Lock file | `.terraform.tfstate.lock.info` (transient, during ops) |
| Backup | `terraform.tfstate.backup` auto-created before overwrite |
| Team risk | Each user has own copy → drift, no shared visibility |

## Backend Block Rules
```
terraform {
  backend "s3" {
    bucket = "my-tf-state"      # literal only
    key    = "prod/terraform.tfstate"
    region = "us-east-1"
  }
}
```
- Exactly ONE backend block per config
- Arguments must be literals (no vars/locals) — use `-backend-config` for partials
- Changing backend config requires `terraform init -migrate-state`

## State Locking
| Backend | Locking Mechanism |
|---|---|
| local | Filesystem lock (same machine only) |
| s3 | DynamoDB table (LockID key, legacy) OR native `use_lockfile = true` (1.10+) |
| Some backends | No locking support at all |

Exam note: locking prevents concurrent writes corrupting state; always confirm backend supports it for team environments.

## Migration Command
```
terraform init -migrate-state
```
Run after changing/adding a backend block on an already-initialized project with existing state.

## State Inspection Commands
| Command | Output |
|---|---|
| `terraform state list` | All resource addresses in state |
| `terraform state show <addr>` | Full attributes for one resource |
| `terraform state pull` | Prints raw remote state JSON to stdout |
| `terraform state mv` | Renames/moves a resource within state |
| `terraform state rm` | Removes a resource from state (not real infra) |

## Sensitive Data Gotcha
- `sensitive = true` hides values from CLI/plan output only
- Full plaintext value still written into state file
- Mitigation: encrypt backend storage (e.g., S3 SSE), restrict IAM access, avoid committing state to VCS

## Common Exam Traps
1. Thinking `sensitive` encrypts state values — it does NOT.
2. Using variables inside a backend block — INVALID, literals only.
3. Forgetting `-migrate-state` after switching backends, assuming `init` alone migrates automatically.
4. Confusing `state list` (all resources) with `state show` (one resource's attributes).
