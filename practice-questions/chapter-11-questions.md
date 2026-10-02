## Chapter 11: Understand HCP Terraform Capabilities — Practice Questions

**Q1.** A company wants to trigger Terraform runs from a custom internal deployment tool that is not Git-based, passing configuration as a payload and polling for run status. Which HCP Terraform run workflow should they use?

A) VCS-driven workflow
B) API-driven workflow
C) CLI-driven workflow
D) Local execution mode

<details>
<summary>Answer</summary>

**Correct: B**

The API-driven workflow lets external tools create workspaces, upload configuration versions, and trigger runs entirely through the HCP Terraform API, without any VCS repo or terraform CLI plan/apply commands.

**Why the others are wrong:**
- VCS-driven requires a connected repository that triggers runs on commit/webhook — not applicable to a custom tool with no Git backend.
- CLI-driven requires developers to run terraform plan/apply locally against the remote backend; it's interactive, not suited for an automated pipeline pushing payloads.
- Local execution mode runs Terraform on the user's own machine and only stores state remotely — it doesn't use HCP Terraform's run engine at all.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q2.** A team needs their AWS credentials rotated automatically per run with no long-lived access keys stored anywhere in HCP Terraform. Which feature satisfies this requirement?

A) Variable sets with sensitive AWS_ACCESS_KEY_ID/SECRET
B) Sentinel policy enforcement
C) Run triggers
D) Dynamic provider credentials

<details>
<summary>Answer</summary>

**Correct: D**

Dynamic provider credentials use OIDC to issue short-lived, workload-identity-based credentials to the AWS/Azure/GCP/Vault provider at run time, eliminating static long-lived secrets entirely.

**Why the others are wrong:**
- Variable sets can store secrets, but sensitive static keys are still long-lived credentials — the exact thing the requirement wants to avoid.
- Sentinel enforces policy on plan output; it has nothing to do with credential issuance.
- Run triggers chain workspace runs together; they don't manage provider authentication.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q3.** A Sentinel policy is set to 'soft-mandatory'. A plan violates this policy. What happens?

A) The run is blocked with no way to proceed
B) The violation is logged but the run proceeds automatically
C) The run is blocked, but a user with override permission can force the apply to proceed
D) The policy is ignored because it's not hard-mandatory

<details>
<summary>Answer</summary>

**Correct: C**

Soft-mandatory policies stop the run by default, but users assigned the 'manage policy overrides' permission can explicitly override the failed check to continue the apply.

**Why the others are wrong:**
- That describes hard-mandatory, which allows no override under any circumstance.
- That describes the advisory level, which only warns without stopping the run.
- Soft-mandatory is still enforced — it's not equivalent to no enforcement at all.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q4.** How does an HCP Terraform workspace differ from a CLI 'terraform workspace' (terraform.tfstate.d)?

A) They are functionally identical, just different names for the same concept
B) CLI workspaces support VCS-driven runs, HCP Terraform workspaces do not
C) An HCP Terraform workspace bundles a full state file, variables, and run history for one configuration, while a CLI workspace is only a named state-file partition within a single backend/config
D) HCP Terraform workspaces only exist for the free tier and are deprecated in paid tiers

<details>
<summary>Answer</summary>

**Correct: C**

An HCP Terraform workspace is a standalone unit containing its own state, variables, permissions, and run history tied to a specific configuration — analogous to a whole working directory, not just a state namespace.

**Why the others are wrong:**
- They share a name but serve very different purposes at very different scopes.
- It's the reverse — VCS-driven runs are an HCP Terraform workspace feature, not a CLI workspace feature.
- CLI workspaces and HCP Terraform workspaces both exist across all tiers; neither statement about deprecation is accurate.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q5.** An organization has 200 workspaces owned by five different application teams. Team leads need scoped access and shared variable sets applied only to their team's workspaces. What is the BEST way to organize this?

A) Create one giant workspace per team containing all their infrastructure
B) Group each team's workspaces into a Project, assign team permissions and variable sets at the project level
C) Use naming conventions only, with org-level teams granted access to all 200 workspaces
D) Duplicate variable sets manually into each of the 200 workspaces

<details>
<summary>Answer</summary>

**Correct: B**

Projects group related workspaces so teams can be granted access and variable sets can be scoped once at the project level, automatically applying to every workspace within it.

**Why the others are wrong:**
- Collapsing infrastructure into one workspace per team removes the isolation and blast-radius benefits of separate state files.
- Naming conventions with org-wide access violate least-privilege and don't scope variable sets automatically.
- Manually duplicating variable sets across 200 workspaces is unmaintainable and exactly what variable sets/projects are designed to avoid.

</details>

---

**Q6.** Which HCP Terraform capability periodically checks a workspace's real infrastructure against its recorded state outside of a normal plan/apply run, without requiring code changes?

A) Run triggers
B) Sentinel policy checks
C) Private module registry versioning
D) Health assessments (drift detection)

<details>
<summary>Answer</summary>

**Correct: D**

Health assessments perform scheduled drift detection, comparing actual infrastructure to the state file and flagging discrepancies even when no new run has been triggered.

**Why the others are wrong:**
- Run triggers chain runs between workspaces on completion; they don't check for drift.
- Sentinel evaluates policy against plan output during a run — it doesn't scan infrastructure independently.
- The module registry manages versioned module publishing, unrelated to state/infrastructure comparison.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q7.** A workspace uses the CLI-driven workflow. Where is the Terraform state stored and where does 'terraform apply' actually execute?

A) State is stored locally, execution happens on HCP Terraform's remote runners
B) Both state and execution remain fully local; HCP Terraform is only used for the UI
C) State is stored remotely, but execution always happens on the developer's local machine
D) State is stored remotely in HCP Terraform, and execution (plan/apply) happens on HCP Terraform's remote runners triggered by the local CLI

<details>
<summary>Answer</summary>

**Correct: D**

In CLI-driven workflow, the developer runs `terraform plan`/`apply` locally, but with the `cloud` block configured, the actual run is streamed to and executed on HCP Terraform's remote infrastructure, with state stored remotely.

**Why the others are wrong:**
- State being local while execution is remote contradicts how remote operations work — state storage and execution are both remote.
- HCP Terraform is far more than a UI in this workflow; it performs the actual remote execution.
- This describes local execution mode, not the CLI-driven remote workflow.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q8.** What is the primary purpose of a private module registry in HCP Terraform?

A) To store and version Terraform provider plugins for offline installation
B) To enforce Sentinel policies on all modules before they can be applied
C) To replace the public Terraform Registry entirely for all providers and modules
D) To publish and version reusable, internally-authored Terraform modules for discovery and consumption across the organization

<details>
<summary>Answer</summary>

**Correct: D**

The private module registry lets teams publish versioned, internally-developed modules (often from VCS tags) so other workspaces in the org can discover and reuse standardized configurations.

**Why the others are wrong:**
- Provider plugin storage/mirroring is a separate concern (provider network mirror), not the module registry's function.
- Policy enforcement is Sentinel/OPA's job, not the module registry's.
- The private registry supplements the public registry for internal modules; it doesn't replace access to public providers/modules.

</details>

---

