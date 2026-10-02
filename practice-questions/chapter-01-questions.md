## Chapter 1: Infrastructure as Code: Concepts Tested on the Exam — Practice Questions

**Q1.** A company manages 200 servers using SSH scripts run manually by engineers, resulting in inconsistent configurations across environments. Which approach BEST solves this problem?

A) Increase change management meetings before manual changes are performed
B) Adopt an IaC tool that provisions infrastructure declaratively from version-controlled code
C) Hire more engineers to perform manual changes faster
D) Use a wiki to document the manual steps for each server

<details>
<summary>Answer</summary>

**Correct: B**

Declarative, version-controlled provisioning removes the root cause of inconsistency by defining and enforcing a single source of truth for infrastructure state.

**Why the others are wrong:**
- More meetings add process overhead but don't stop manual, error-prone execution.
- More people performing manual changes still produces click-ops-style inconsistency.
- Documentation describes intended steps but doesn't enforce or verify actual state.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q2.** A team runs 'terraform apply' twice in a row without modifying any code between runs. What should happen on the second apply?

A) Terraform destroys and recreates all resources
B) Terraform reports no changes, since actual state matches desired state
C) Terraform reports an error because the resources already exist
D) Terraform creates duplicate copies of all resources

<details>
<summary>Answer</summary>

**Correct: B**

Idempotency means applying the same configuration repeatedly produces the same result; if nothing changed, Terraform reports the infrastructure is already up to date.

**Why the others are wrong:**
- There's no reason to destroy/recreate resources when the config is unchanged.
- Terraform compares state rather than erroring on existing resources.
- Duplicating resources would violate the declarative, state-tracked model.

</details>

---

**Q3.** A company runs workloads on both AWS and Azure and wants a single tool with consistent syntax to provision resources on both. Which Terraform capability enables this?

A) Terraform's imperative scripting engine
B) Terraform's provider plugin architecture, which abstracts each cloud's API behind a common configuration language
C) Terraform Cloud's built-in cost estimation feature
D) Terraform's local state file locking mechanism

<details>
<summary>Answer</summary>

**Correct: B**

Providers are plugins that translate HCL resource blocks into calls against a specific platform's API, letting the same core workflow target many clouds.

**Why the others are wrong:**
- Terraform's core language is declarative, not imperative.
- Cost estimation is unrelated to enabling multi-cloud provisioning.
- State locking supports safe collaboration, not multi-cloud capability.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q4.** Which of the following BEST describes a purely imperative approach to infrastructure provisioning?

A) Writing a Terraform configuration file specifying the desired end state of resources
B) Defining resources in HCL and letting Terraform determine the execution order automatically
C) Using a state file to track only the differences between desired and current configuration
D) Writing a sequence of CLI commands that must run in a specific order to create resources step-by-step

<details>
<summary>Answer</summary>

**Correct: D**

Imperative provisioning means specifying the exact ordered steps ('how') needed to reach a result, as opposed to declaring the desired outcome ('what').

**Why the others are wrong:**
- Describes the declarative model, not imperative.
- Also describes declarative behavior, since Terraform determines ordering itself.
- Describes state tracking mechanics, not the imperative provisioning style.

</details>

---

**Q5.** Which Terraform command compares current infrastructure state against configuration to detect drift, without making any changes?

A) terraform apply
B) terraform destroy
C) terraform plan
D) terraform validate

<details>
<summary>Answer</summary>

**Correct: C**

'terraform plan' produces a read-only diff between the desired configuration and the real infrastructure state, surfacing drift without applying changes.

**Why the others are wrong:**
- 'apply' actually makes changes to reach the desired state.
- 'destroy' removes resources rather than detecting drift.
- 'validate' only checks configuration syntax, not the real infrastructure's current state.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q6.** An engineer manually deletes a security group rule that Terraform created, outside of Terraform. What will 'terraform plan' show on the next run?

A) No changes, because Terraform doesn't track manual changes
B) An error stating the resource no longer exists, halting all execution
C) A prompt asking whether to import the manual change into state
D) A diff indicating Terraform will recreate/re-add the missing rule to match configuration

<details>
<summary>Answer</summary>

**Correct: D**

Terraform compares its tracked state/config against the real resource and detects the drift, proposing to reconcile the infrastructure back to the declared configuration.

**Why the others are wrong:**
- Terraform state includes that rule, so drift is detected, not ignored.
- Plan reports the discrepancy as a proposed change rather than halting with an error.
- No automatic import prompt occurs; import is a separate explicit command.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q7.** A team wants to require peer review of all infrastructure changes before they reach production. Which practice, enabled by IaC, best supports this?

A) Manually emailing screenshots of console changes to a manager for approval
B) Storing Terraform configurations in a version control system and requiring pull request reviews before merge
C) Restricting console access to a single administrator account
D) Running terraform apply directly against production with no review step

<details>
<summary>Answer</summary>

**Correct: B**

Version-controlled configuration plus a pull-request review workflow is the core IaC-enabled collaboration and code-review benefit.

**Why the others are wrong:**
- Manual screenshot approval is unscalable and not an IaC-native practice.
- Restricting console access limits collaboration but doesn't provide code review.
- Applying with no review step defeats the goal entirely.

</details>

---

**Q8.** Which of the following is NOT a benefit typically associated with Infrastructure as Code?

A) Guaranteed zero-downtime deployments for every resource type
B) Repeatable, consistent provisioning across environments
C) Ability to track infrastructure changes via version control history
D) Reduced configuration drift between environments

<details>
<summary>Answer</summary>

**Correct: A**

IaC does not guarantee zero downtime; downtime depends on the resource type and architecture, not on merely defining infrastructure as code.

**Why the others are wrong:**
- Repeatability across environments is a well-documented, core IaC benefit.
- Version control history/auditability is a core IaC benefit.
- Drift reduction/detection is a core, heavily tested IaC benefit.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

