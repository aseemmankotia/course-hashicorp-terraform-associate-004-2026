## Chapter 3: Terraform Fundamentals: Providers and State Basics — Practice Questions

**Q1.** A configuration must manage AWS resources in both us-east-1 and eu-west-1 using a single AWS provider source. What is the correct way to define the second regional configuration?

A) Add a second required_providers entry named 'aws-eu' pointing to the same source
B) Set the region argument as a list inside a single provider block
C) Use two separate .tf files each declaring 'provider "aws"' without aliases
D) Add a second 'provider "aws"' block with a different alias argument, then reference it in resources with the provider meta-argument

<details>
<summary>Answer</summary>

**Correct: D**

Multiple configurations of the same provider require the alias argument inside additional provider blocks; resources then select the config via provider = aws.alias_name.

**Why the others are wrong:**
- required_providers only declares source/version constraints, not per-region configuration — it cannot hold a second named instance like 'aws-eu'
- The region argument accepts a single string, not a list, inside one provider block
- Terraform merges all provider blocks with the same local name across files; two un-aliased blocks for the same provider produce a duplicate configuration error

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q2.** Given `required_providers { aws = { source = "hashicorp/aws", version = "~> 5.10" } }`, which version will terraform init select if 5.10.0, 5.10.4, 5.11.0, and 6.0.0 are all available?

A) 6.0.0, the newest available
B) 5.11.0, since ~> allows the last two components to float
C) 5.10.0 only, exact pin regardless of patches
D) 5.10.4, the latest patch/minor version within the 5.10.x line

<details>
<summary>Answer</summary>

**Correct: D**

~> 5.10 (two-part form) locks the leftmost specified component (5) and allows the rightmost (10) to increment only within patch releases of that same minor version, so it resolves to the highest 5.10.x — here 5.10.4.

**Why the others are wrong:**
- 6.0.0 is excluded because ~> restricts upgrades to the specified version segment, not major versions
- 5.11.0 is excluded — ~> 5.10 only floats the patch digit, not the minor digit; that behavior applies to ~> 5 instead
- 5.10.0 ignores that ~> permits patch upgrades; an exact pin would use '= 5.10.0'

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q3.** A developer adds a new provider to required_providers and commits the change. A teammate pulls the change and runs `terraform init`, receiving an error that the lock file does not match the configuration. What should the teammate run to resolve this correctly?

A) terraform apply -auto-approve to force provider installation
B) Delete .terraform.lock.hcl entirely and never commit it again
C) terraform state pull to refresh provider metadata
D) terraform init -upgrade to recalculate and update the lock file with the new provider's hashes

<details>
<summary>Answer</summary>

**Correct: D**

terraform init -upgrade recalculates version selections and hashes for all providers, updating .terraform.lock.hcl to include the newly added provider while respecting version constraints.

**Why the others are wrong:**
- apply does not resolve lock file mismatches; init must succeed first
- The lock file should be committed to version control — deleting it removes the guarantee of consistent provider versions across team members and CI
- state pull retrieves the state file, which has nothing to do with provider dependency locking

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q4.** What is the primary purpose of the .terraform.lock.hcl file?

A) It stores the current values of all resource attributes for drift detection
B) It records the exact provider versions and cryptographic hashes selected during init, ensuring consistent provider installations across runs and machines
C) It locks the state file to prevent concurrent terraform apply operations
D) It caches provider binaries locally to speed up subsequent init calls

<details>
<summary>Answer</summary>

**Correct: B**

The dependency lock file pins the selected provider version and checksum hashes so that terraform init installs identical provider versions every time, regardless of which machine or CI runner executes it.

**Why the others are wrong:**
- Resource attribute values and drift detection are the state file's job, not the lock file's
- State locking (preventing concurrent applies) is a separate mechanism, typically implemented via backend locking (e.g., DynamoDB), unrelated to .terraform.lock.hcl
- The plugin cache directory (TF_PLUGIN_CACHE_DIR) stores binaries; the lock file only stores version/hash metadata, not the binaries themselves

</details>

---

**Q5.** A company wants terraform init to reuse previously downloaded provider plugins across multiple projects instead of re-downloading them each time. Which mechanism accomplishes this?

A) Adding a 'cache' block inside required_providers
B) Running terraform init with the -reconfigure flag
C) Committing the .terraform directory to version control
D) Setting the TF_PLUGIN_CACHE_DIR environment variable to a shared local directory

<details>
<summary>Answer</summary>

**Correct: D**

Setting TF_PLUGIN_CACHE_DIR points Terraform to a shared cache directory; init checks it first and symlinks cached provider binaries instead of re-downloading them, saving bandwidth and time across projects.

**Why the others are wrong:**
- There is no 'cache' block syntax inside required_providers — plugin caching is configured via environment variable or CLI config file, not HCL blocks
- -reconfigure only affects backend re-initialization, not provider plugin caching
- Committing .terraform (which includes provider binaries) is discouraged; it is large, platform-specific, and regenerated by init

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q6.** Which required_providers declaration correctly pins the AWS provider to any 4.x version but excludes 5.0 and above?

A) aws = { source = "hashicorp/aws", version = ">= 4.0" }
B) aws = { source = "hashicorp/aws", version = "= 4" }
C) aws = { source = "hashicorp/aws", version = "~> 4.0" }
D) aws = { source = "hashicorp/aws", version = "< 5.0" }

<details>
<summary>Answer</summary>

**Correct: C**

~> 4.0 (single trailing zero after the major digit) is shorthand meaning >= 4.0, < 5.0 — it allows any 4.x minor/patch release but blocks the next major version.

**Why the others are wrong:**
- >= 4.0 alone has no upper bound and would permit 5.0 or higher
- = 4 is not valid version syntax and does not express a range
- < 5.0 alone has no lower bound and would permit versions like 3.0 or 0.5

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q7.** What role does the Terraform state file play in a working configuration?

A) It stores the .tf source code as a backup in case files are deleted
B) It defines which provider versions are allowed to be installed
C) It maps resources declared in configuration to the real-world infrastructure objects Terraform manages, tracking metadata and dependencies between runs
D) It replaces the need for a required_providers block entirely

<details>
<summary>Answer</summary>

**Correct: C**

The state file is Terraform's source of truth linking each configured resource block to its actual deployed object (via IDs and attributes), enabling plan/apply to detect drift and compute accurate diffs.

**Why the others are wrong:**
- State does not back up .tf source files — it stores resource metadata, not HCL source
- Provider version constraints live in required_providers/lock file, not in state
- required_providers is still mandatory to declare source and version constraints; state has no bearing on that

</details>

---

**Q8.** A resource block includes `provider = aws.secondary`. What must exist elsewhere in the configuration for this to be valid?

A) A provider "aws" block containing `alias = "secondary"`
B) A required_providers entry with source = "hashicorp/aws" named 'secondary'
C) A second variable block defining 'secondary' as a region string
D) A backend configuration block referencing the secondary alias

<details>
<summary>Answer</summary>

**Correct: A**

Resources reference a specific provider configuration using provider = <local_name>.<alias>; this alias must be defined via the alias argument in an additional provider "aws" block.

**Why the others are wrong:**
- required_providers entries are keyed by local provider name (e.g., 'aws'), not by alias — aliases are declared only in provider blocks
- A variable is unrelated to how Terraform resolves provider aliasing; it wouldn't create the aws.secondary reference
- Backend blocks configure state storage, not provider instance selection

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

