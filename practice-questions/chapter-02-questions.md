## Chapter 2: Terraform vs Other IaC & Configuration Tools — Practice Questions

**Q1.** A company uses Terraform to provision AWS EC2 instances but also needs to install packages, configure users, and manage application config files on those instances after boot. Which approach BEST fits Terraform's intended positioning?

A) Use Terraform to provision the infrastructure, then hand off to Ansible for in-instance configuration management
B) Use Terraform's built-in remote-exec provisioner for all ongoing configuration management
C) Replace Terraform entirely with Ansible dynamic inventory to both provision and configure
D) Write custom Terraform providers that replicate Ansible modules for package installation

<details>
<summary>Answer</summary>

**Correct: A**

Terraform is a provisioning tool that manages infrastructure lifecycle and state; Ansible is agentless configuration management better suited for ongoing in-instance tasks like package installs and user config. The standard pattern is Terraform provisions, Ansible configures.

**Why the others are wrong:**
- remote-exec provisioners are explicitly discouraged by HashiCorp as a last resort — they are not idempotent and break Terraform's declarative model
- Ansible has no native state tracking or dependency graph for cloud resource provisioning the way Terraform does — using it alone loses drift detection and plan preview
- Building custom providers to duplicate Ansible's configuration modules is unnecessary engineering effort when integration is the standard pattern

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q2.** A DevOps engineer runs `terraform plan` before applying changes to a production VPC. What does the output of this command represent?

A) A confirmation that the changes have already been applied to the real infrastructure
B) A dry-run execution log showing errors from the last apply
C) A preview of the actions Terraform will take to reconcile current state with the desired configuration, without making any changes
D) A rollback script that can undo the last successful apply

<details>
<summary>Answer</summary>

**Correct: C**

terraform plan compares the current state file against the desired configuration and produces an execution plan — additions, changes, destructions — with zero side effects on real infrastructure.

**Why the others are wrong:**
- plan never modifies real infrastructure; only apply does
- plan is not an error log from a previous run — it evaluates the current diff
- Terraform has no built-in automatic rollback script generation from plan output

</details>

---

**Q3.** A configuration defines an AWS security group and an EC2 instance that references that security group's ID via an attribute reference. During `terraform apply`, how does Terraform determine the order in which these resources are created?

A) Terraform builds a resource dependency graph and creates the security group before the instance because of the implicit reference
B) Resources are created in the exact top-to-bottom order they appear in the .tf file
C) Terraform creates resources alphabetically by resource type
D) Terraform creates all resources in parallel regardless of references, then patches missing IDs afterward

<details>
<summary>Answer</summary>

**Correct: A**

Terraform parses implicit and explicit dependencies to build a directed acyclic resource graph, ensuring dependent resources (like the security group) are created before resources that reference them.

**Why the others are wrong:**
- File order is irrelevant to Terraform's execution order — it relies entirely on the graph
- Terraform does not order by alphabetical resource type; ordering is dependency-driven
- Parallelism only applies to resources with no interdependency; dependent resources are sequenced, not patched after the fact

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q4.** Which characteristic differentiates a purely procedural configuration tool from Terraform's declarative model?

A) Procedural tools execute a defined sequence of steps to reach an outcome, while Terraform's declarative model specifies the desired end state and lets Terraform determine the necessary steps
B) Procedural tools require you to specify the desired end state, and the tool determines the steps
C) Declarative tools like Terraform require you to write step-by-step imperative instructions to reach a state
D) There is no meaningful difference; both approaches always produce identical execution plans

<details>
<summary>Answer</summary>

**Correct: A**

Declarative tools describe the desired end state (what), and the tool computes the steps (how). Procedural/imperative tools require explicit ordered instructions (how) to reach that state.

**Why the others are wrong:**
- This reverses the definitions — procedural means step-by-step instructions, not desired-state declarations
- This also reverses the definitions — Terraform is declarative, not imperative
- The two approaches differ fundamentally in authoring style and how changes are computed, so this is false

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q5.** A team wants to find a community-published module for provisioning a standardized VPC on AWS instead of writing one from scratch. Where should they look first?

A) The Ansible Galaxy
B) The AWS Systems Manager Parameter Store
C) The HashiCorp Vault Secrets Engine catalog
D) The Terraform Registry

<details>
<summary>Answer</summary>

**Correct: D**

The Terraform Registry hosts providers and community/verified modules, including reusable VPC modules for AWS, Azure, and GCP.

**Why the others are wrong:**
- Ansible Galaxy hosts Ansible roles/collections, not Terraform modules or providers
- Parameter Store is for storing configuration values/secrets, not IaC modules
- Vault's secrets engines manage dynamic secrets and credentials, unrelated to Terraform module distribution

</details>

---

**Q6.** An engineer wants to save a Terraform execution plan to a file so that the exact reviewed plan is applied later without re-evaluating configuration drift. Which command produces this?

A) terraform plan -out=tfplan
B) terraform apply -auto-approve
C) terraform state pull > tfplan
D) terraform validate -save=tfplan

<details>
<summary>Answer</summary>

**Correct: A**

The -out flag on terraform plan saves the computed execution plan to a binary file, which can later be passed directly to terraform apply tfplan to apply exactly what was reviewed.

**Why the others are wrong:**
- apply -auto-approve applies changes immediately without a saved reviewable plan file
- state pull retrieves the current state, not a plan of proposed changes
- terraform validate only checks syntax/configuration validity and has no -save flag

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q7.** Which CLI command generates a visual representation of the resource dependency graph in a Terraform configuration?

A) terraform show
B) terraform graph
C) terraform validate --graph
D) terraform plan --tree

<details>
<summary>Answer</summary>

**Correct: B**

terraform graph outputs a DOT-format representation of the resource dependency graph, which can be rendered visually with Graphviz to show ordering and relationships.

**Why the others are wrong:**
- terraform show displays the current state or a saved plan in human-readable form, not the dependency graph
- validate does not have a --graph flag; it only checks configuration syntax and internal consistency
- plan does not have a --tree flag; the graph command is separate and dedicated to visualization

</details>

---

**Q8.** How does a provisioning tool like Terraform fundamentally differ from an agent-based configuration management tool like Chef?

A) Terraform requires an installed agent on every managed node, while Chef is completely agentless
B) Terraform tracks the state of provisioned infrastructure resources and manages their lifecycle, while agent-based tools like Chef continuously enforce configuration state on already-running nodes via an installed agent
C) Both tools serve an identical purpose and are fully interchangeable for every use case
D) Terraform can only manage on-premises hardware, while Chef can only manage cloud resources

<details>
<summary>Answer</summary>

**Correct: B**

Terraform is a state-driven provisioning tool focused on creating, updating, and destroying infrastructure resources, tracked via a state file. Chef is agent-based configuration management that continuously enforces desired configuration on running nodes via a client/agent checking in with a server.

**Why the others are wrong:**
- This reverses the actual architecture — Chef requires an agent (chef-client), Terraform does not
- The tools solve different problems (provisioning vs configuration enforcement) and are commonly used together, not interchangeably
- Neither tool is restricted to on-prem or cloud only — both can target either environment depending on providers/cookbooks used

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

