## Chapter 7: Writing HCL: Types, Loops, Validation & Sensitive Data — Practice Questions

**Q1.** A team manages 5 EC2 instances using count = 5. Instances 4 and 5 hold stateful data created by local-exec provisioners. The team must remove the 3rd instance without triggering destroy/recreate on instances 4 and 5. Which change accomplishes this safely?

A) Refactor the resource to use for_each with a map keyed by a stable instance name
B) Reduce count to 4 on the existing resource block
C) Run terraform state rm on index [2] only and reapply
D) Add lifecycle { prevent_destroy = true } to the resource block

<details>
<summary>Answer</summary>

**Correct: A**

for_each addresses resources by a stable key (e.g. resource.name["web3"]) instead of a numeric index, so removing one key only affects that instance — the others are untouched by Terraform's plan.

**Why the others are wrong:**
- Reducing count shifts every index above the removed one down by one, causing Terraform to destroy and recreate instances 4 and 5.
- Manually removing state entries desyncs state from config and still leaves the count-based addressing problem for future changes.
- prevent_destroy blocks any destroy on that resource including intentional ones, but does not fix the index-shifting behavior of count.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q2.** A variable is declared with sensitive = true and used to configure a resource attribute. A colleague claims the secret is now fully protected end-to-end. Which statement correctly evaluates this claim?

A) False — sensitive = true only redacts the value from CLI/UI output; it is still written in plaintext in the state file unless the backend encrypts it
B) True — sensitive values are automatically encrypted inside the state file
C) True — Terraform never persists sensitive values to state
D) False — sensitive values are automatically routed to a HashiCorp Vault-managed file

<details>
<summary>Answer</summary>

**Correct: A**

sensitive = true suppresses the value in plan/apply console output and error messages, but Terraform still stores the resolved value in the state file in plaintext; protecting state (encrypted backend, access controls) is a separate responsibility.

**Why the others are wrong:**
- State encryption is not automatic — it depends on the backend configuration (e.g., S3 with SSE, Terraform Cloud encryption-at-rest).
- Terraform does persist sensitive values to state; the flag only affects displayed output.
- There is no automatic routing to Vault; Vault integration requires explicit provider configuration.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q3.** You must attach a variable number of ingress rules to a single aws_security_group resource, driven by a list variable of port/CIDR pairs. Which HCL feature generates the repeated nested ingress blocks inside that one resource?

A) The count meta-argument applied to the aws_security_group resource
B) A for expression assigned directly to the ingress argument
C) A dynamic block that iterates over the list and emits a content block per element
D) A separate module invoked with for_each for every port

<details>
<summary>Answer</summary>

**Correct: C**

dynamic blocks are purpose-built to generate repeated nested configuration blocks (like ingress) inside a single resource, iterating over a list, set, or map and rendering a content block per iteration.

**Why the others are wrong:**
- count operates on whole resource instances, not on nested blocks within one resource.
- ingress expects a block structure, not a plain list value from a for expression — this would fail to apply.
- Splitting into a module per port is unnecessary overhead and doesn't address generating nested blocks within one resource.

</details>

---

**Q4.** You need to combine var.default_tags and var.extra_tags into a single map, where keys present in extra_tags override matching keys in default_tags. Which function should you use?

A) lookup(var.default_tags, var.extra_tags)
B) join(",", [var.default_tags, var.extra_tags])
C) concat(var.default_tags, var.extra_tags)
D) merge(var.default_tags, var.extra_tags)

<details>
<summary>Answer</summary>

**Correct: D**

merge() combines multiple maps into one; when the same key exists in more than one argument, the value from the later argument wins, matching the override requirement.

**Why the others are wrong:**
- lookup() retrieves a single value from a map by key with an optional default — it does not merge two maps.
- join() concatenates list elements into a string using a delimiter; it does not operate on maps.
- concat() combines lists, not maps, and would error on map inputs.

</details>

---

**Q5.** You need to retrieve a value from a map by key, supplying a fallback default when the key does not exist, without causing an error. Which function fits this requirement?

A) lookup(map, key, default)
B) element(list, index)
C) merge(map1, map2)
D) coalesce(val1, val2)

<details>
<summary>Answer</summary>

**Correct: A**

lookup(map, key, default) returns the value for the given key, or the supplied default if the key is absent, avoiding an error from a missing map index.

**Why the others are wrong:**
- element() works on lists by numeric index, not maps by key.
- merge() combines multiple maps together; it doesn't perform a single key lookup with a default.
- coalesce() returns the first non-null argument from a list of values, unrelated to map key lookups.

</details>

---

**Q6.** Which HCL block correctly enforces, at plan time, that var.env must be one of "dev", "staging", or "prod"?

A) variable "env" { type = string; validation { condition = var.env in ["dev","staging","prod"]; error_message = "invalid env" } }
B) variable "env" { type = string; precondition { condition = contains(["dev","staging","prod"], var.env) } }
C) variable "env" { type = string; validation { condition = contains(["dev","staging","prod"], var.env); error_message = "env must be dev, staging, or prod." } }
D) check "env_check" { assert { condition = var.env } }

<details>
<summary>Answer</summary>

**Correct: C**

Variable validation blocks require a condition expression and an error_message; contains() is the correct function to test list membership in HCL.

**Why the others are wrong:**
- HCL has no 'in' operator for list membership; this syntax is invalid.
- precondition blocks are only valid inside a lifecycle block on resource, data, or output blocks — not directly inside variable blocks.
- A check block's assert requires a boolean condition expression and error_message, not a bare variable reference, and check blocks validate assumptions post-plan, not variable input directly.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q7.** Where can a precondition block be defined to validate an assumption before a resource is created or updated?

A) Directly inside a variable block
B) Inside a provider block
C) Inside a lifecycle block within a resource, data, or output block
D) Inside the top-level terraform block

<details>
<summary>Answer</summary>

**Correct: C**

precondition and postcondition blocks (Terraform 1.2+) are nested inside a lifecycle block on resource, data source, or output blocks to assert conditions before or after evaluation.

**Why the others are wrong:**
- Variable-level constraints use validation blocks, not precondition/postcondition.
- Provider blocks configure provider settings and do not support precondition blocks.
- The terraform block configures required_version/backend/providers and has no lifecycle or condition support.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q8.** Which statement best describes the conceptual integration between Terraform and HashiCorp Vault for secrets management?

A) Terraform automatically stores every sensitive variable inside Vault at apply time with no provider configuration required
B) The vault provider lets Terraform read secrets dynamically at plan/apply time, but the resulting values are still persisted in Terraform state and must be protected independently
C) Vault replaces the need for a remote backend to store Terraform state
D) Setting sensitive = true on a variable automatically routes its value through Vault

<details>
<summary>Answer</summary>

**Correct: B**

The vault provider (via data sources like vault_generic_secret) fetches secrets at runtime, but Terraform still writes resolved values into state, so state encryption/access control remains necessary regardless of Vault usage.

**Why the others are wrong:**
- There is no automatic mechanism that writes Terraform variables into Vault; integration requires explicit provider and data source configuration.
- Vault manages secrets, not Terraform state storage — a backend (S3, Terraform Cloud, etc.) is still required for state.
- sensitive = true only affects CLI/UI output redaction; it has no relationship to Vault routing.

</details>

---

