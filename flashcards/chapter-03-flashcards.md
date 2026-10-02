## Chapter 3: Terraform Fundamentals: Providers and State Basics — Flashcards

| # | Front (Question) | Back (Answer) |
|---|-----------------|---------------|
| 1 | required_providers syntax — what two arguments does each provider entry need? | source (e.g. "hashicorp/aws") and version (constraint string). Exam tip: source is required starting with Terraform 0.13+ to disambiguate providers not published by HashiCorp. |
| 2 | Version constraint operator: ~> 5.10 (two-part) | Allows patch upgrades only: >= 5.10.0, < 5.11.0. Exam tip: number of trailing components determines what can float — the rightmost one only. |
| 3 | Version constraint operator: ~> 5.0 (single trailing zero) | Allows minor+patch upgrades: >= 5.0.0, < 6.0.0. Exam tip: fewer specified components = wider allowed range. |
| 4 | Version constraint operators available in Terraform | =, !=, >, <, >=, <=, and ~> (pessimistic operator). Exam tip: multiple constraints can be combined, e.g. ">= 4.0, < 5.0". |
| 5 | Purpose of .terraform.lock.hcl | Records exact provider versions + checksums selected during init to guarantee consistent installs across machines/CI. Exam tip: SHOULD be committed to version control. |
| 6 | When does .terraform.lock.hcl update? | When running terraform init -upgrade, when a new provider is added to required_providers, or when constraints change and a different version must be selected. Plain 'terraform init' won't change it if the lock already satisfies constraints. |
| 7 | How to configure a shared provider plugin cache? | Set TF_PLUGIN_CACHE_DIR environment variable to a directory. Terraform init symlinks providers from cache instead of re-downloading. Exam tip: not configured via HCL, only via env var or CLI config file. |
| 8 | Provider alias — purpose | Lets you define multiple configurations of the same provider (e.g., different AWS regions) via alias = "name" in extra provider blocks. Resources reference it with provider = aws.name. |
| 9 | Default provider config vs aliased provider config | A provider block without alias is the default used automatically by resources with no provider argument. Aliased blocks are only used when explicitly referenced. |
| 10 | What does the Terraform state file map? | Configuration resource blocks to real infrastructure object IDs/attributes. Exam tip: state is the sole source of truth for plan/apply diffing — not the provider or lock file. |
| 11 | terraform init — provider installation source order | Checks: 1) plugin cache dir (if set), 2) local mirror/filesystem mirror config, 3) registry (default registry.terraform.io). Exam tip: source attribute like "hashicorp/aws" resolves against registry.terraform.io/hashicorp/aws. |
| 12 | Exact version pin syntax | version = "= 5.10.0" (or just "5.10.0"). Exam tip: omitting the operator defaults to exact match equivalent to '='. |
