## Chapter 11: Understand HCP Terraform Capabilities — Flashcards

| # | Front (Question) | Back (Answer) |
|---|-----------------|---------------|
| 1 | HCP Terraform workspace vs CLI 'terraform workspace' — key difference? | HCP Terraform workspace = full unit with its own state, variables, permissions, and run history for one config. CLI workspace = just a named state partition within one backend/config. Exam tip: they share a name, not a purpose. |
| 2 | Three HCP Terraform run workflows | VCS-driven (webhook on commit), CLI-driven (local plan/apply, remote execution), API-driven (external tool calls API directly). Exam tip: API-driven = CI/CD integration without VCS or CLI. |
| 3 | Sentinel enforcement levels (3) | Advisory = warn only, run proceeds. Soft-mandatory = blocks run, override permission can bypass. Hard-mandatory = blocks run, no override possible. |
| 4 | What are Projects in HCP Terraform used for? | Group related workspaces for scoped team access (RBAC) and to attach variable sets at project level instead of per-workspace. Exam tip: use Projects when multiple teams own separate sets of workspaces. |
| 5 | What are Variable Sets? | Reusable collections of variables (including sensitive ones) that can be assigned to specific workspaces, a whole Project, or the entire organization — avoids duplicating vars per workspace. |
| 6 | Dynamic provider credentials — what problem do they solve? | Issue short-lived, OIDC-based credentials to providers (AWS/Azure/GCP/Vault) at run time instead of storing static long-lived access keys as workspace variables. |
| 7 | Sentinel vs OPA in HCP Terraform | Sentinel = HashiCorp's native policy-as-code language. OPA = open-source alternative using Rego. Both integrate as policy checks in the run pipeline before apply. Choose based on org's existing policy tooling. |
| 8 | What is a Run Trigger? | A configured link where a completed run in one workspace automatically queues a new run in a downstream (dependent) workspace — used to chain infrastructure layers (e.g., network -> app). |
| 9 | Health Assessments / Drift Detection | Periodic scheduled check comparing real infrastructure to recorded state without needing a new plan/apply. Flags drift so teams can reconcile before it causes issues. |
| 10 | Private Module Registry purpose | Publish and version internally-authored reusable modules (often synced from VCS tags) so other workspaces/teams can discover and consume them consistently. |
| 11 | CLI-driven workflow — where does execution happen? | Developer runs terraform plan/apply locally, but with the `cloud` block configured, execution streams to and runs on HCP Terraform's remote runners; state stored remotely. Exam trap: assuming execution stays local. |
| 12 | Teams/RBAC — typical workspace permission levels | Read, Plan, Write, Admin (plus custom permission sets). Applied via Teams assigned at workspace or project scope. Exam tip: least-privilege — grant Plan/Write, reserve Admin for workspace owners only. |
