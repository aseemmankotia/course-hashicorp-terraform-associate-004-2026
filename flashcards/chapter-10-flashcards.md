## Chapter 10: Drift, Refactoring State & Import — Flashcards

| # | Front (Question) | Back (Answer) |
|---|-----------------|---------------|
| 1 | Terraform version that introduced the moved block? | 1.1. Exam tip: moved block maps old state address to new config address, avoiding destroy/recreate on refactor. |
| 2 | Terraform version that introduced the removed block? | 1.7. Exam tip: default behavior destroys the resource (destroy=true); set lifecycle { destroy = false } to only forget it from state. |
| 3 | Terraform version that introduced -refresh-only plan/apply mode? | 0.15.4. Exam tip: use plan -refresh-only to preview drift, apply -refresh-only to persist reconciled state with zero infrastructure changes. |
| 4 | moved block required arguments | from and to. Exam tip: no other arguments are valid inside a moved block. |
| 5 | removed block default lifecycle behavior (no destroy argument specified) | destroy = true — infrastructure IS destroyed, same as deleting the resource block. Exam tip: set destroy = false explicitly to keep infra running. |
| 6 | terraform state rm vs removed block — which is reviewable in a plan? | removed block — appears in terraform plan output and lives in version control. terraform state rm executes immediately via CLI with no plan step. |
| 7 | terraform import command syntax | terraform import <resource_address> <resource_id>. Exam tip: requires a matching resource block already in config; import only writes to state, it does not generate config unless using 1.5+ import blocks with generate-config-out. |
| 8 | Terraform version that introduced declarative import blocks with generate-config-out? | 1.5. Exam tip: import blocks let you batch multiple imports and optionally auto-generate .tf config, unlike the legacy terraform import CLI command. |
| 9 | terraform state mv use case | Immediately renames/moves a resource address within the state file via CLI. Exam tip: unlike moved block, it's imperative and not tracked in config/version history. |
| 10 | terraform plan -refresh-only vs standard terraform plan | -refresh-only reports ONLY drift (state vs real infra), proposing no create/update/destroy actions. Standard plan also proposes corrective changes to reconcile config with reality. |
| 11 | Does terraform apply -refresh-only change real infrastructure? | No. It only updates the state file to match real-world resources; infrastructure is left untouched. |
| 12 | Risk of deleting a resource from config without a removed block? | Terraform plans to DESTROY the real infrastructure on next apply — same outcome as removed block's default (destroy=true), just not reviewable/traceable as a named refactor operation. |
