## Chapter 4: Core Workflow Part 1: Init, Validate, Plan — Flashcards

| # | Front (Question) | Back (Answer) |
|---|-----------------|---------------|
| 1 | What three main things does `terraform init` do? | Downloads provider plugins, initializes/configures the backend, and installs referenced modules. Exam tip: init must be re-run after adding a new provider or module, or changing backend config. |
| 2 | What file does `terraform init` create to lock provider versions, and should it be committed to VCS? | .terraform.lock.hcl — yes, commit it. It records exact provider versions/hashes selected so all team members and CI get identical providers. Exam tip: not the same as terraform.tfstate. |
| 3 | Does `terraform validate` contact provider APIs or the backend? | No. It performs only static syntax and internal consistency checks (e.g., variable/resource references exist, types match). Exam tip: it can run without credentials or network access, unlike plan. |
| 4 | terraform plan symbol: `+` | Resource will be created. Exam tip: memorize all four symbols — + create, - destroy, ~ update in-place, -/+ destroy then create (replace). |
| 5 | terraform plan symbol: `-/+` vs `+/-` | -/+ = destroy then create (default replace order). +/- = create then destroy (used when create_before_destroy = true). Exam tip: order swap signals lifecycle block usage. |
| 6 | terraform plan symbol: `~` | Resource will be updated in-place — no destroy/recreate needed. Exam tip: contrast with -/+ which requires replacement due to a ForceNew-style attribute change. |
| 7 | How do you save a plan for later, guaranteed-exact apply? | terraform plan -out=tfplan, then later terraform apply tfplan. Exam tip: applying the saved file skips the interactive yes/no prompt and applies exactly what was reviewed. |
| 8 | What happens if you run `terraform apply tfplan` after state has changed since the plan was saved? | Terraform errors out with a 'stale plan' message and refuses to apply — you must re-run terraform plan. Exam tip: this is a commonly-missed gotcha about saved plan safety. |
| 9 | Does `terraform plan` refresh state by default? | Yes — plan reads real infrastructure to detect drift before computing the diff, unless run with -refresh=false. Exam tip: refresh-only mode (terraform plan -refresh-only) updates state without proposing resource changes. |
| 10 | What is a 'speculative plan'? | A plan that cannot be applied — e.g. a plan run without -out, or a VCS/PR-triggered plan in HCP Terraform without apply rights. Exam tip: used for safe review-only workflows in CI/PR checks. |
| 11 | Flag to skip backend initialization during terraform init | -backend=false. Exam tip: useful for local testing of provider/module config without touching remote state; must re-init normally before any real apply. |
| 12 | Flag to change backend configuration and migrate existing state to it | terraform init -migrate-state. Exam tip: contrast with -reconfigure, which switches backend config WITHOUT migrating existing state data. |
