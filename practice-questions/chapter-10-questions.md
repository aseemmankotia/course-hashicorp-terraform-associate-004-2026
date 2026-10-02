## Chapter 10: Drift, Refactoring State & Import — Practice Questions

**Q1.** Your team renamed a resource in configuration from aws_db_instance.primary to aws_db_instance.production and moved it into a new module structure. The underlying RDS instance must not be destroyed or recreated. Which approach correctly updates Terraform's tracking of this resource?

A) Run terraform state rm aws_db_instance.primary, then terraform import into the new address
B) Add a moved block specifying from = aws_db_instance.primary and to = module.db.aws_db_instance.production
C) Delete the old resource block and apply; Terraform will detect the RDS instance is unchanged
D) Use terraform taint aws_db_instance.primary followed by terraform apply

<details>
<summary>Answer</summary>

**Correct: B**

moved block declaratively maps the old state address to the new one, so Terraform updates its tracking during plan/apply with no destroy or recreate.

**Why the others are wrong:**
- state rm + import works but requires two manual, unreviewed steps and re-specifying the resource ID; it's not the exam-preferred refactor pattern and risks a gap in tracking.
- Without a moved block or state operation, Terraform sees the old address gone and the new address as new, planning destroy + create.
- taint forces recreation on the next apply — the opposite of the desired outcome.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q2.** A resource in your Terraform-managed environment must be removed from Terraform state and configuration, but the actual infrastructure (a shared storage bucket now owned by another team) must remain running. Your team is on Terraform 1.7 and requires all state changes to be reviewable in a plan before being applied. Which approach meets this requirement?

A) Add a removed block with lifecycle { destroy = false } and remove the resource block from config
B) terraform state rm aws_s3_bucket.shared
C) Delete the resource block from configuration and run terraform apply
D) Add a removed block with lifecycle { destroy = true }

<details>
<summary>Answer</summary>

**Correct: A**

A removed block with destroy = false appears in terraform plan output, is version-controlled, and forgets the resource from state without destroying it.

**Why the others are wrong:**
- state rm executes immediately with no plan step and isn't recorded in version control.
- Deleting the resource block from config without a removed block causes Terraform to plan destroying the bucket.
- destroy = true (the removed block default) would actually destroy the bucket, violating the requirement.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q3.** Your production environment may have experienced manual out-of-band changes (e.g., someone changed an EC2 instance type via console). You want Terraform to detect the difference and update its state to reflect reality, without modifying config or applying any changes to infrastructure. Which command achieves this?

A) terraform plan
B) terraform apply -refresh-only
C) terraform refresh
D) terraform import

<details>
<summary>Answer</summary>

**Correct: B**

-refresh-only apply mode updates the state file to match real infrastructure after you review the refresh-only plan, with zero changes to infrastructure or config.

**Why the others are wrong:**
- A standard plan refreshes state internally but proposes config-driven create/update/destroy actions, not a pure drift reconciliation.
- terraform refresh performs the update but without the reviewable plan step that the -refresh-only workflow provides.
- import brings a new, untracked resource under management; it doesn't reconcile drift on already-tracked resources.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q4.** You need to review exactly which already-tracked resources have drifted from their configuration, without generating a plan that would create, update, or destroy resources. Which command should you run first?

A) terraform validate
B) terraform plan -refresh-only
C) terraform plan
D) terraform state list

<details>
<summary>Answer</summary>

**Correct: B**

-refresh-only plan mode compares state to real infrastructure and reports only drift, without proposing corrective config actions.

**Why the others are wrong:**
- validate only checks configuration syntax, not infrastructure drift.
- A normal plan reports drift but bundles it with proposed remediating changes, not a pure drift report.
- state list only lists tracked resource addresses; it never compares against real infrastructure.

</details>

---

**Q5.** An existing Azure storage account was created manually in the portal and must now be brought under Terraform management with minimal risk of Terraform planning to modify or recreate it on the next apply. What must be true before running terraform import?

A) A resource block with matching type and matching arguments must already exist in configuration
B) The storage account must have no tags applied
C) The Terraform state file must be deleted first
D) The provider block must include a lifecycle { prevent_destroy = true } argument

<details>
<summary>Answer</summary>

**Correct: A**

terraform import only populates state; it requires an existing resource block whose arguments closely match the real resource so the following plan shows no unintended diff.

**Why the others are wrong:**
- Tags are unrelated to the import prerequisite.
- Deleting the state file would remove tracking of every other resource; it's unnecessary and destructive.
- prevent_destroy is an unrelated lifecycle safeguard, not an import requirement.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q6.** Which two arguments are required inside a moved block?

A) id and provider
B) source and destination
C) resource and address
D) from and to

<details>
<summary>Answer</summary>

**Correct: D**

moved blocks use from = <old_address> and to = <new_address> to map prior state addresses to new configuration addresses.

**Why the others are wrong:**
- id and provider are not valid moved block arguments.
- source and destination are not valid moved block argument names.
- resource and address are not recognized moved block arguments; Terraform will error.

</details>

---

**Q7.** By default, if you write a removed block for a resource with no lifecycle argument specified, what does terraform plan propose?

A) The resource is only forgotten from state, infrastructure is untouched
B) The resource is destroyed, matching normal removal behavior
C) Terraform throws an error requiring the destroy argument
D) The resource is imported back into state automatically

<details>
<summary>Answer</summary>

**Correct: B**

The removed block's default lifecycle behavior is destroy = true, equivalent to simply deleting the resource block from config — infrastructure is destroyed unless destroy = false is explicitly set.

**Why the others are wrong:**
- That state-only behavior requires explicitly setting lifecycle { destroy = false }.
- removed blocks are valid without a lifecycle argument; destroy = true is simply the default, no error occurs.
- removed blocks never re-import a resource; they only remove or destroy it.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q8.** What is the key difference between terraform state mv and a moved block for renaming a resource address?

A) state mv only works for data sources, moved blocks only work for managed resources
B) state mv changes the state file immediately via CLI with no plan review; a moved block is declared in config and applied as part of a reviewable plan/apply
C) moved blocks require deleting the .tfstate file first; state mv does not
D) There is no functional difference; both are deprecated in favor of removed blocks

<details>
<summary>Answer</summary>

**Correct: B**

state mv is an imperative, immediate CLI state edit outside version control review; a moved block is declarative, versioned in configuration, and reviewed during the normal plan/apply workflow.

**Why the others are wrong:**
- Both operate on managed resources and modules; the distinction isn't data sources versus managed resources.
- Neither operation requires deleting the state file.
- moved and removed blocks serve different purposes (rename vs. remove) and neither is deprecated; state mv still has valid ad-hoc uses.

</details>

---

