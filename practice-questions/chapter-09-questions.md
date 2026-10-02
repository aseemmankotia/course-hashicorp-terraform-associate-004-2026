## Chapter 9: Terraform State Fundamentals & Remote Backends — Practice Questions

**Q1.** A team of 6 engineers currently runs terraform apply from their own laptops using the default local backend. Deployments frequently create duplicate resources and drift because each engineer has a different terraform.tfstate file. Which change BEST solves this for the team going forward?

A) Have engineers coordinate manually over Slack before running apply
B) Add a local-exec provisioner that copies state to a shared folder after every apply
C) Configure a remote backend (e.g. S3 with native locking or Terraform Cloud) so all engineers read/write a single shared state with locking
D) Run terraform apply -lock=false to avoid conflicts

<details>
<summary>Answer</summary>

**Correct: C**

A remote backend centralizes state so every team member reads/writes the same file, and supported backends provide locking to serialize writes — this directly fixes both drift (stale local copies) and concurrent-write corruption.

**Why the others are wrong:**
- Manual coordination doesn't scale and doesn't fix the fundamental problem of separate state copies
- Copying files after the fact still allows two people to write simultaneously before the copy happens — no locking guarantee
- -lock=false disables the safety mechanism entirely and increases corruption risk, the opposite of what's needed

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q2.** You add a backend block to configure an S3 remote backend on an existing project that previously used the local backend with real infrastructure already deployed. What must you run to move the existing state into S3?

A) terraform init -migrate-state
B) terraform apply -auto-approve
C) terraform state pull
D) terraform import

<details>
<summary>Answer</summary>

**Correct: A**

terraform init -migrate-state detects the backend configuration change and offers to copy existing state into the new backend location, preserving resource tracking.

**Why the others are wrong:**
- apply doesn't migrate state, it just plans/applies changes against whatever backend is currently configured
- state pull only downloads a copy of remote state to stdout, it doesn't perform migration
- import is used to bring an unmanaged existing resource under Terraform management, not to move state between backends

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q3.** Which of the following is a valid backend block for configuring a remote S3 backend?

A) terraform { backend "s3" { bucket = "my-tf-state" key = "prod/terraform.tfstate" region = "us-east-1" } }
B) terraform { backend "s3" { bucket = var.bucket_name region = "us-east-1" } }
C) provider "s3" { backend = true bucket = "my-tf-state" }
D) resource "terraform_backend" "s3" { bucket = "my-tf-state" }

<details>
<summary>Answer</summary>

**Correct: A**

backend blocks nest inside a terraform block, use a hardcoded backend type string, and only accept literal values for arguments — this option follows valid syntax and rules.

**Why the others are wrong:**
- Backend block arguments cannot reference variables, locals, or resource attributes — only literal values or -backend-config partial configuration
- There is no such thing as a backend attribute inside a provider block
- Backends are not configured as resources; this syntax does not exist

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q4.** A developer runs terraform state show aws_instance.web to inspect a resource. What does this command actually display?

A) The HCL configuration block that defines the resource
B) The detailed attributes currently recorded in state for that single resource instance
C) The full list of every resource address currently tracked in state
D) A diff between the current state and the live infrastructure

<details>
<summary>Answer</summary>

**Correct: B**

terraform state show <address> prints the full set of attribute values Terraform has recorded in state for that one specific resource instance.

**Why the others are wrong:**
- That's the .tf configuration file content, not what state show returns
- That description matches terraform state list, not state show
- A live diff is what terraform plan produces, not state show

</details>

---

**Q5.** A security review flags that a database password, defined as a resource argument and marked sensitive in the Terraform configuration, is discoverable in terraform.tfstate. Why does this happen?

A) Terraform automatically encrypts sensitive-marked values before writing them to state
B) Sensitive values are only stored in state if remote backend encryption is disabled
C) This is a bug and should be reported to HashiCorp
D) State stores all resource attribute values, including those marked sensitive, in plaintext by default

<details>
<summary>Answer</summary>

**Correct: D**

The sensitive flag only suppresses values from CLI output (plan/apply); Terraform still writes the actual value into the state file in plaintext, by design.

**Why the others are wrong:**
- There is no automatic encryption of sensitive attributes within the state file content itself
- Encryption of the backend storage medium (e.g., S3 SSE) is unrelated to whether the value is plaintext within the state document itself
- This is documented, expected behavior, not a defect

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q6.** Which statement correctly describes state locking behavior across backend types?

A) All Terraform backends support locking identically using a hidden hash algorithm
B) Only the local backend supports locking; all remote backends require locking to be disabled
C) Locking is only relevant for terraform destroy, not terraform apply
D) Locking support and mechanism vary by backend type — for example S3 can use DynamoDB or native lockfile-based locking, while some backends provide no locking at all

<details>
<summary>Answer</summary>

**Correct: D**

Locking is backend-specific: local backend uses a filesystem lock file, S3 can use a DynamoDB lock table or the newer native S3 conditional-write lockfile mechanism, and some backend types do not support locking at all.

**Why the others are wrong:**
- Locking mechanisms differ significantly between backend types, they are not identical
- Local backend locking only protects against concurrent runs on the same machine; many remote backends also support locking
- Locking protects any state-modifying operation, including apply, destroy, and even some plan operations that refresh state

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q7.** What is the primary purpose of the Terraform state file?

A) It stores a compiled binary version of the .tf configuration for faster execution
B) It caches provider API authentication tokens between runs
C) It stores a copy of every past terraform plan output for auditing
D) It maps resources declared in configuration to real-world object IDs and records their last-known attribute values

<details>
<summary>Answer</summary>

**Correct: D**

State is the source of truth mapping each managed resource block to an actual infrastructure object (via ID) and its recorded attributes, which Terraform uses to compute diffs on the next plan.

**Why the others are wrong:**
- Terraform does not compile HCL into a binary; it's parsed at runtime each execution
- Provider credentials are supplied via provider configuration/environment, not cached in state
- State is not a plan history log; it reflects only the current known state, not historical plans

</details>

---

**Q8.** A company must decide how to store Terraform state for a multi-region production environment with 15 engineers. Which backend configuration BEST meets requirements for durability, team collaboration, and locking?

A) Local backend with state files committed to a shared Git repository
B) A local NFS-mounted folder shared across engineer workstations
C) Storing state as a Kubernetes ConfigMap without a lock provider
D) S3 bucket with versioning enabled, encryption at rest, and locking enabled via DynamoDB or native S3 lockfile support

<details>
<summary>Answer</summary>

**Correct: D**

S3 with versioning, encryption, and a supported locking mechanism satisfies durability (versioned/immutable history), collaboration (shared centralized backend), and safe concurrency (locking) — the standard production pattern.

**Why the others are wrong:**
- Committing state to Git exposes sensitive plaintext data in history and provides no locking, risking merge conflicts and corruption
- NFS mounts don't guarantee atomic writes or reliable locking across all client OS implementations
- ConfigMaps have size limits and no built-in Terraform locking support without additional configuration

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

