## Chapter 4: Core Workflow Part 1: Init, Validate, Plan — Practice Questions

**Q1.** A DevOps engineer runs `terraform plan -out=tfplan`, saves the file, and asks a teammate to review it. Two hours later, another engineer applies an unrelated change to the same state via a separate pipeline. The original engineer then runs `terraform apply tfplan`. What happens?

A) Terraform silently applies the stale plan, overwriting the other engineer's change
B) Terraform automatically re-runs plan and merges both sets of changes
C) Terraform apply fails with an error that the plan is stale and refuses to apply it
D) Terraform apply succeeds but rolls back the other engineer's change without warning

<details>
<summary>Answer</summary>

**Correct: C**

Terraform compares the saved plan's serial/lineage against the current state. If state has changed since the plan was generated, apply fails with a 'Saved plan is stale' error and requires a fresh `terraform plan`.

**Why the others are wrong:**
- Terraform does not silently apply against changed state — it detects staleness and errors out
- Terraform never automatically merges plans; it requires a new plan to be generated
- There is no automatic rollback behavior in Terraform's apply workflow

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q2.** A team wants pull-request CI checks to show proposed infrastructure changes for review, but must guarantee those changes cannot be applied directly from the CI job. Which workflow satisfies this requirement?

A) Run `terraform apply -auto-approve` in the CI job and revert on failure
B) Run `terraform plan -out=tfplan` and store the file as a CI artifact for later use in the same job
C) Run `terraform plan` without `-out`, producing a speculative plan that has no apply path
D) Run `terraform validate` only, since it also shows planned resource changes

<details>
<summary>Answer</summary>

**Correct: C**

A plan run without `-out` (or a plan run in a context without apply permissions, such as a VCS-triggered PR check in HCP Terraform) is a speculative plan — it shows proposed changes but there is no saved plan file to apply.

**Why the others are wrong:**
- Auto-approving apply in CI directly applies changes, violating the requirement
- Saving a plan file with -out creates an artifact that COULD be applied elsewhere, defeating the safety goal for a pure review check
- terraform validate never computes or displays resource changes — it only checks syntax and internal consistency

</details>

---

**Q3.** During `terraform apply` of a saved plan file (`terraform apply tfplan`), how does Terraform handle refreshing infrastructure state compared to a normal `terraform plan`?

A) It performs an additional refresh before applying to catch last-minute drift
B) It uses the state as captured at plan time and does not refresh again before applying
C) It always prompts the user to choose whether to refresh
D) It ignores the saved plan and re-computes the diff from scratch

<details>
<summary>Answer</summary>

**Correct: B**

Once a plan is saved with -out, the actions in that plan are fixed. Applying the saved plan file does not re-refresh state — it executes exactly what was planned, which is why a stale plan produces an error instead of silently re-planning.

**Why the others are wrong:**
- An extra refresh would contradict the purpose of a saved plan (executing an exact, reviewed set of actions)
- There is no interactive refresh prompt when applying a saved plan file
- Terraform does not discard a valid saved plan and recompute — it errors if state has diverged instead

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q4.** Which `terraform init` flag prevents Terraform from initializing or contacting the configured backend, useful for local testing without remote state access?

A) -input=false
B) -migrate-state
C) -lock=false
D) -backend=false

<details>
<summary>Answer</summary>

**Correct: D**

`terraform init -backend=false` skips backend initialization entirely, useful when only providers/modules need to be downloaded without touching remote state.

**Why the others are wrong:**
- -input=false disables interactive prompts, it does not affect the backend
- -migrate-state is used when changing backend configuration, it still initializes a backend
- -lock=false disables state locking during operations, unrelated to backend initialization

</details>

---

**Q5.** Which flag on `terraform plan` skips reading the real infrastructure state and computes the diff purely from the last recorded state file?

A) -out=false
B) -refresh=false
C) -lock=false
D) -destroy

<details>
<summary>Answer</summary>

**Correct: B**

By default, `terraform plan` refreshes state (reads current real-world resource attributes) before computing the diff. `-refresh=false` skips this step and plans against the last-known state file only.

**Why the others are wrong:**
- -out=false is not a valid flag syntax; -out expects a filename to save the plan
- -lock=false disables state locking, unrelated to the refresh step
- -destroy generates a plan to destroy all resources, it does not control refresh behavior

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q6.** Which command checks configuration syntax and internal consistency (e.g., referenced variables and resources exist, argument types match) without contacting any provider APIs or the backend?

A) terraform validate
B) terraform plan
C) terraform apply -refresh-only
D) terraform init -upgrade

<details>
<summary>Answer</summary>

**Correct: A**

terraform validate performs static analysis of the configuration: syntax and internal consistency checks. It does not call out to provider APIs, read real infrastructure, or require valid credentials.

**Why the others are wrong:**
- terraform plan does contact providers and may refresh real infrastructure state
- terraform apply -refresh-only reads real infrastructure to update state and requires provider access
- terraform init -upgrade downloads/upgrades providers, it is not a syntax check command

</details>

---

**Q7.** In `terraform plan` output, a resource shows the symbol `-/+` next to it. What does this indicate?

A) The resource will be updated in-place with no downtime
B) The resource will be destroyed and then a new one created to replace it
C) The resource will be created and immediately marked for deletion by a later run
D) The resource has no changes and will be left untouched

<details>
<summary>Answer</summary>

**Correct: B**

`-/+` means the resource must be replaced: Terraform will destroy the existing resource, then create a new one, because a change requires a property that cannot be updated in place (e.g., ForceNew attribute).

**Why the others are wrong:**
- In-place update with no destroy is represented by `~`, not `-/+`
- Create followed by delete order is `+/-` (used with create_before_destroy = true), not `-/+`
- No changes are shown with no symbol/action at all for that resource in the plan summary

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q8.** What is the key functional difference between `terraform validate` and `terraform plan`?

A) validate checks pricing implications while plan only checks syntax
B) validate requires cloud credentials while plan does not
C) There is no functional difference; plan is simply a verbose alias for validate
D) validate performs only static, offline checks; plan additionally contacts providers/backend and computes an actual proposed diff against real or recorded state

<details>
<summary>Answer</summary>

**Correct: D**

validate is a purely local, static check of syntax and internal consistency. plan goes further: it initializes provider connections, may refresh state, and produces a concrete diff of proposed create/update/destroy actions.

**Why the others are wrong:**
- Neither command evaluates cost/pricing information
- It's the reverse — plan is the one that typically requires credentials to talk to providers, not validate
- They are distinct commands serving different purposes in the workflow, not aliases

</details>

---

