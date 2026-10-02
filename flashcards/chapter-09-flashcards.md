## Chapter 9: Terraform State Fundamentals & Remote Backends — Flashcards

| # | Front (Question) | Back (Answer) |
|---|-----------------|---------------|
| 1 | What is stored in the local backend's default state filename? | terraform.tfstate in the working directory. Exam tip: this is the default when no backend block is configured — no locking across machines, only local file locks. |
| 2 | What flag migrates existing state into a newly configured backend? | terraform init -migrate-state — prompts to copy current state into the new backend target. Exam tip: required whenever the backend block changes on an initialized project. |
| 3 | Can backend block arguments reference variables or locals? | No — backend configuration only accepts literal values (or partial config via -backend-config). Exam tip: a backend block using var.xxx is INVALID and a common trap answer. |
| 4 | How many backend blocks are allowed per Terraform configuration? | Exactly one. Exam tip: multiple backend blocks in the same config is a syntax error. |
| 5 | terraform state list vs terraform state show — difference? | state list = all resource addresses tracked in state. state show <address> = full attribute detail for ONE resource. Exam tip: exam often swaps these in distractor answers. |
| 6 | Is sensitive-marked data encrypted inside the state file itself? | No — it's stored in plaintext regardless of the sensitive flag in config. Exam tip: sensitive only hides values from CLI output, not from the state document. |
| 7 | How does S3 backend locking work (modern Terraform)? | Either a DynamoDB table (legacy, primary key LockID) or native S3 conditional-write locking via use_lockfile = true (Terraform 1.10+, no DynamoDB required). Exam tip: know both mechanisms exist. |
| 8 | What lock file does the local backend create during operations? | .terraform.tfstate.lock.info — appears only while an operation is running, protects against concurrent local writes on the same machine. |
| 9 | Why does state locking matter for teams? | Prevents two simultaneous writes (e.g., two applies) from corrupting or overwriting state. Exam tip: without locking, concurrent applies can cause lost updates or duplicated resources. |
| 10 | What does Terraform automatically create before overwriting state on an operation? | terraform.tfstate.backup — a copy of the prior state, used for manual recovery. Exam tip: not a substitute for versioned remote backend history. |
| 11 | Name three supported remote backend types (conceptually). | S3, azurerm (Azure Blob Storage), gcs (Google Cloud Storage) — also kubernetes, http, pg, consul. Exam tip: know these exist even if config syntax isn't memorized verbatim. |
| 12 | Why does Terraform need a state file at all? | To map declared config resources to real infrastructure object IDs and cache last-known attribute values, enabling accurate diffs on the next plan. Exam tip: without state, Terraform can't know what already exists. |
