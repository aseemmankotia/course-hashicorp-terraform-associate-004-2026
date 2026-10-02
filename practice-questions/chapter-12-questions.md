## Chapter 12: Full Practice Exam & 60-Minute Time-Management Strategy — Practice Questions

**Q1.** The TA-004 exam gives you roughly 60 minutes for approximately 57 questions. You are 40 minutes in and have answered 45 questions, with 4 flagged for review. What is the BEST use of your remaining time?

A) Immediately re-read every answered question from the start to double-check accuracy
B) Skip the flagged questions entirely and submit early to avoid running out of time
C) Finish the remaining unanswered questions first, then use leftover time to revisit the 4 flagged ones
D) Spend the remaining time only on the flagged questions and leave any unanswered questions blank

<details>
<summary>Answer</summary>

**Correct: C**

Every question is worth points, so finishing all questions guarantees no free points are lost to blanks; only after all questions have an answer should you spend remaining time on flagged items.

**Why the others are wrong:**
- Re-reading everything wastes time and increases risk of not finishing the unanswered questions
- Skipping flagged questions abandons partial credit opportunity when time remains
- Leaving unanswered questions blank guarantees zero points on those instead of a guessed chance at credit

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q2.** A configuration currently uses `count` to create three identical EC2-style compute instances. The team later needs to add a fourth instance in the middle of the list without destroying and recreating the other three. What change should be made?

A) Keep `count` and simply increase the number from 3 to 4
B) Add a `moved` block instead of changing the resource block
C) Switch the resource to `for_each` with a map or set of unique keys
D) Use `terraform import` to add the fourth instance manually

<details>
<summary>Answer</summary>

**Correct: C**

for_each uses a stable key-based index, so inserting a new item doesn't shift the index of existing resources, avoiding unwanted destroy/recreate of unrelated instances.

**Why the others are wrong:**
- count uses a numeric index, so inserting an item in the middle of the list shifts every subsequent index and can trigger destroy/recreate
- A moved block only relocates existing state addresses; it does not solve the underlying indexing problem for new insertions
- terraform import brings an existing real-world resource into state; it does not restructure how the count/for_each list is indexed

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q3.** What is the primary purpose of the `.terraform.lock.hcl` file?

A) It records the exact provider versions and checksums selected so runs are consistent across team members and CI
B) It stores the current state of deployed infrastructure
C) It prevents concurrent `terraform apply` runs against the same state file
D) It stores sensitive variable values so they aren't committed to version control

<details>
<summary>Answer</summary>

**Correct: A**

The dependency lock file records selected provider versions and hashes so that terraform init produces the same provider set on every machine, and it should be committed to version control.

**Why the others are wrong:**
- Infrastructure state is stored in the state file (terraform.tfstate), not the lock file
- State locking to prevent concurrent applies is handled by the backend's locking mechanism (e.g., DynamoDB table), not this file
- Sensitive values are handled via variables/tfvars and secret managers, not the provider lock file

</details>

---

**Q4.** A candidate is comparing Terraform Community Edition (open-source CLI) with HCP Terraform for a final-day review. Which statement correctly distinguishes the two?

A) Community Edition includes Sentinel policy-as-code enforcement built in, while HCP Terraform does not
B) Community Edition requires a paid subscription while HCP Terraform is entirely free with no limitations
C) HCP Terraform replaces HCL configuration language with a graphical-only workflow
D) HCP Terraform is a managed service that adds remote state storage, remote runs, a private registry, and policy enforcement on top of the open-source core, while Community Edition is the local CLI without those managed features

<details>
<summary>Answer</summary>

**Correct: D**

HCP Terraform (formerly Terraform Cloud) is HashiCorp's managed platform layered on the open-source Terraform CLI, adding remote execution, remote state, private module registry, and governance features like Sentinel policies.

**Why the others are wrong:**
- Sentinel policy-as-code is a paid HCP Terraform/Enterprise feature, not part of the open-source CLI
- Community Edition (the CLI) is free and open source; it's HCP Terraform that has paid tiers beyond a limited free tier
- HCP Terraform still uses standard HCL configuration; it adds a UI/API layer but does not replace HCL

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q5.** During a refactor, a team renames a resource block from `aws_instance.web` to `aws_instance.app_server` without wanting Terraform to destroy and recreate the underlying infrastructure. Which approach is BEST?

A) Manually edit the state file with a text editor to change the resource address
B) Add a `moved` block declaring the from/to addresses so Terraform tracks the rename in state automatically on the next plan/apply
C) Delete the resource from configuration and re-add it under the new name
D) Run `terraform destroy` on the old name and `terraform apply` to create the new name

<details>
<summary>Answer</summary>

**Correct: B**

The moved block is the supported, declarative way to tell Terraform that a resource's configuration address changed, so it updates state in place instead of planning a destroy/create.

**Why the others are wrong:**
- Manually editing the state file is unsupported and risky; terraform state mv or a moved block are the correct mechanisms
- Deleting and re-adding under a new name with no moved block causes Terraform to plan a destroy of the old and create of the new
- Explicitly destroying and recreating causes real infrastructure downtime/recreation, which is exactly what the moved block avoids

</details>

---

**Q6.** A resource was previously managed by Terraform but the team now wants Terraform to stop managing it WITHOUT destroying the real infrastructure. Which Terraform 1.7+ feature is designed for this?

A) The `moved` block
B) `terraform destroy -target`
C) Deleting the resource block and running `terraform apply`
D) The `removed` block with `lifecycle { destroy = false }` (forget-only) behavior

<details>
<summary>Answer</summary>

**Correct: D**

The removed block lets you declare that a resource should be removed from Terraform's management/state while explicitly controlling whether the real infrastructure is destroyed, supporting a 'forget' workflow.

**Why the others are wrong:**
- moved is for renaming/refactoring addresses in state, not for dropping management of a resource
- terraform destroy -target explicitly destroys the real infrastructure, which is the opposite of the forget-only goal
- Simply deleting the block and applying plans a destroy of the resource, removing the real infrastructure

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q7.** On exam day, remote proctoring for HashiCorp certifications is handled through Certiverse. Which of the following is a correct logistics reminder for this format?

A) A valid government-issued photo ID and a room scan showing a clear desk are typically required before the exam begins
B) You may keep a printed cheat sheet next to your keyboard as long as it's out of camera view
C) You are allowed unlimited bathroom breaks with the timer paused automatically
D) A second monitor may be used to keep reference documentation open during the exam

<details>
<summary>Answer</summary>

**Correct: A**

Remote-proctored certification exams (via Certiverse) generally require photo ID verification and a webcam room scan to confirm a clear desk and no unauthorized materials before the timer starts.

**Why the others are wrong:**
- Printed materials of any kind, in or out of camera view, are prohibited during a closed-book proctored exam
- Breaks are typically not paused-timer accommodations by default and may require prior approval or count against exam time
- Secondary monitors and reference material access are disallowed in a closed-book proctored certification exam

</details>

---

**Q8.** With about 60 minutes for roughly 57 questions, which pacing strategy is MOST appropriate?

A) Spend up to 5 minutes on any question you're unsure about to guarantee it's correct before moving on
B) Answer only the questions you're 100% confident about and leave the rest blank
C) Budget about one minute per question, flag anything taking longer or causing doubt, and move on immediately to protect time for a final review pass
D) Read every question twice before answering to avoid misreading, regardless of time impact

<details>
<summary>Answer</summary>

**Correct: C**

With roughly one minute per question available, the effective strategy is fast triage: answer what you know quickly, flag uncertain items, and preserve a buffer at the end to revisit flagged questions rather than getting stuck.

**Why the others are wrong:**
- Spending 5 minutes on a single question can consume the buffer needed for several other questions and flagged review
- Leaving low-confidence questions blank forfeits any chance of partial/guessed credit compared to flagging and answering with best guess
- Reading every question twice by default adds unnecessary time; it should be reserved for genuinely ambiguous questions only

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

